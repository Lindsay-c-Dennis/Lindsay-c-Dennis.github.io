---
layout: post
title:      "Using Jquery and AJAX to render without redirects"
date:       2018-05-16 18:40:37 +0000
permalink:  using_jquery_and_ajax_to_render_without_redirects
---

I had a perfectly functional rails app, and then I had to go and start putting jquery all over it.


I implemented js features on several different pages of my app, but the bulk of the functionality happens on landmarks show pages, relating to reviews associated with those landmarks. Each landmark show page displays a landmark name, picture, description, neighborhood, category, and a link to an outside website with more information. Below all of this is a list of the three most recent reviews that users have made of the landmark, along with buttons to see more reviews or add a new one. In its original incarnation, the buttons at the bottom of the page redirected to new pages. The “add review” button would take the user to a separate page with a form for creating a review. Upon submission, the user was redirected back to the landmark show page, where the new review would appear among the most recent reviews. The “more reviews” button redirected the user to a reviews index page set up to display all reviews associated with that landmark. It was functional, but a bit clumsy.

With jquery and javascript, I was able to implement similar functionality without the redirects. 

First, I set up an event handler for the new review button: 
```
$(document).on('click', "#add-review", function(e){
		e.preventDefault();
		const newUrl = this.href
		$.get(`${newUrl}`, function(data) {
			$('#new-review-form').html(data);
		});
	});
```
This function uses preventDefault() to hijack the button and prevent it from performing a redirect. It uses the “this” keyword to capture the redirect destination, and uses that url to perform an ajax get request with the convenient jquery $.get method. From there, it takes the data response from the get request and inserts it into a div on the show page with the id of “new-review-form”. On the controller side, I tweaked the new method to render just the html that I wanted to insert, which was contained in a partial: 
```
def new 
		@landmark = Landmark.find(params[:landmark_id])
		@review = Review.new(user_id: current_user.id, landmark_id: @landmark.id)
		render partial: 'form', locals: { review: @review, landmark: @landmark }
		
	end
```
Because reviews are nested under landmarks, I also had to create instance variables to pass to the partial for a successful render.

With this code, the add review button automatically inserts a review form into the page. I set up another event handler to grab the user submitted data and render it directly to the page: 

```
$(document).on('submit', '#new_review', function(e){
		e.preventDefault(); 
		let url = $(this).attr('action')
		let data = $(this).serializeArray();
		$.post(url, data).done(function(review) {
			newRev = new Review(review.id, review.user.id, review.user.name, review.landmark.id, review.landmark.name, review.content, review.created_at, review.cu);
			$('#landmark-reviews').append(newRev.renderReview());
			$('#new-review-form').empty();
		});
	});

```
Again, the function starts by preventing the default action. It declares a local variable and assigns it the value of the action attribute of the new review form, and another variable to hold the data from the form, using the serializeArray() method. A jquery $.post method makes a post request with the user’s data from the form. In the post request, it creates a new Review object using ES6 class methods and assigns it to a variable. It uses ajquery selector to grab the landmark-reviews div and append some html. This html was created by the prototype method renderReview() that I built on the Review object (code below). Finally, it empties the div that held the new review form. 

Here are my class constructor and prototype methods: 
```
class Review {
		constructor(reviewId, userId, userName, landmarkId, landmarkName, content, createdAt, cu) {
			this.reviewId = reviewId
			this.userId = userId;
			this.userName = userName
			this.landmarkId = landmarkId;
			this.landmarkName = landmarkName
			this.content = content
			this.createdAt = createdAt
			this.currentUser = cu
		}
	}

	Review.prototype.renderReview = function() {
			let postTime = moment(this.createdAt).format('LLL')
			let reviewBody = `
				<h3> On <a href='/landmarks/${this.landmarkId}'>${this.landmarkName}</a>, ${this.userName} says: </h3>
				<p>${this.content}</p>
				<h6><em>Review posted ${postTime}</em></h6>
				
		<br /> `
			let buttons = ''
			if (this.currentUser.id === this.userId) {
				buttons = `
					<a href="/landmarks/${this.landmarkId}/reviews/${this.reviewId}/edit", data-id="${this.reviewId}", class="edit-review btn btn-default btn-xs">Edit</a>
					<a href="/users/${this.userId}/reviews/${this.reviewId}", data-id="${this.reviewId}", class="delete-review btn btn-danger btn-xs">Delete</a>`
			}
			return reviewBody + buttons;
		}	

```
This method renders html that matches the already listed reviews, including edit and delete buttons which appear when the current user is the author of the comment.

The 'more reviews' button uses similar functionality to display all reviews for the landmark on the show page:

```
$(document).on('click', '#more-reviews', function(e) {
		e.preventDefault();
		$('#landmark-reviews').empty();
		const url = this.href
		$.get(`${url}.json`, function(reviews) {
			reviews.forEach(review => {
				const viewReview = new Review(review.id, review.user.id, review.user.name, review.landmark.id, review.landmark.name, review.content, review.created_at, review.cu)
				$('#landmark-reviews').append(viewReview.renderReview());
			})
		});
	});

```
Again, this function hijacks the click event and captures the target url in a variable. It immediately empties the landmark-reviews div so that no reviews appear twice. It then uses the target url to make a get request (specified as json, because my controller is set up to respond to both html and json requests) to the index route, receiving a response of a json object holding all reviews for this landmark. It then uses the .forEach method to call the renderReview prototype method on each review and append it to the div.

With jquery and ajax, I was able to manipulate the transfer of information through HTTP request and manipulate the DOM to update without page refreshes, creating a faster app with a more comfortable user experience.

