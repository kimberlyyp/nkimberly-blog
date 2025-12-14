---
layout: post
title: "Brew Crew, Beer Review Blog"
date: 2017-10-08 02:19:31 +0000
categories: ['Web Development']
---

### Background
Brew Crew is a Create, Read, Update, and Destroy application that was built using the Model-View-Controller design pattern and proper Test-Driven Development. At Brew Crew, users can sign up and sign in to create, comment, favorite, and vote on beer reviews as well as receive updates on comments on their favorites through email.

[youtube https://www.youtube.com/watch?v=L1C49cqqcSA&=853&h=480]

[View the repo](https://github.com/n-kimberly/bloccit).
### Project Goals
My goal withBrew Crew was to incorporate the nine user stories designated by Bloc's Web Developer Bootcamp curriculum. In doing so, I sought to improve my overall competency with developing using proper test-driven development, designing an application in proper Model-View-Controller pattern, seeding and querying databases to update views, implementing all fundamental aspects of Create, Read, Update, and Destroy, and seamlessly integrating all of these features by leveraging the rails framework.

User Stories:

	Users can sign-up by providing a username, password and email.
	Users can sign in and out of Brew Crew.
	User roles available: admin or standard user.
	Users can create new posts and edit posts they own, while admins can delete and create any topic or post.
	Users can comment on posts.
	Users can up/down vote any post.
	Users can favorite any post and be emailed with updates on that post.
	User's profile displays their posts and comments.
	Development database is seeded automatically with users, topics and posts.

### Process
I started off this project by setting up my development environment. This included ruby, Rspec, rails and all the tools necessary for building and deploying software.

**1. Users can sign-up by providing a username, password and email.**
*User Model*
In order to allow signups, we must be able to store and validate user data. User data can be stored and validated via a User model.

Abiding by proper TDD, I first drafted up all of my requirements of this model. This includes, validating the presence and minimum length of a username, validating the uniqueness and length of a user email, and ensuring a secure password.

I confirm that the tests fail before writing my User model. Then I generated my User model, wrote in my validations code, installed BCrypt to handle the complex hashing algorithm to encrypt our passwords, and confirmed that all tests pass.
 *Users Controller*
Next up, I generated a Users controller. The controller receives user input and makes calls to models and the views to perform desired actions. My Users controller includes a new method and a create method.

The new method calls on the new view to allow users to input their information and store it in the params, while the create method saves the user's input and passes it to the sessions controller.* These methods pass my tests, which ask for successful http status, redirects, and user instantiation with all of the proper data.

**2. Users can sign in and out of Brew Crew.**
*Sessions Controller*
In order to keep a user logged in and handle sign in's and sign out's, we create a Sessions Controller. Sessions are tracked and persisted by storing values in the client's cookies. We want to be able to create a new session (signing in) as well as destroy that session (signing out).

[sourcecode language="ruby" wraplines="true" collapse="true"]
require 'rails_helper'

RSpec.describe SessionsController, type: :controller do

  let(:my_user) {
    create(:user)
  }

  describe "GET new" do
    it "returns http success" do
      get :new
      expect(response).to have_http_status(:success)
    end
  end

  describe "POST create" do
    it "returns http success" do
      post :create, params: {
        session: {
          email: my_user.email
        }
      }
      expect(response).to have_http_status(:success)
    end
    it "initializes a session" do
      post :create, params: {
        session: {
          email: my_user.email,
          password: my_user.password
        }
      }
      expect(session[:user_id]).to eq(my_user.id)
    end
    it "does not add user id to session if password missing" do
      post :create, params: {
        session: {
          email: my_user.email
        }
      }
      expect(session[:user_id]).to be_nil
    end
    it "flashes error when email is not valid" do
      post :create, params: {
        session: {
          email: "does not exist"
        }
      }
      expect(flash.now[:alert]).to be_present
    end
    it "renders #new with bad email address" do
      post :create, params: {
        session: {
          email: "does not exist"
        }
      }
      expect(response).to render_template(:new)
    end
    it "when successful, redirects to the root view" do
      post :create, params: {
        session: {
          email: my_user.email,
          password: my_user.password
        }
      }
      expect(response).to redirect_to(root_path)
    end
  end

  describe "DELETE sessions/id" do
    it "renders the #welcome view" do
      delete :destroy, params: {
        id: my_user.id
      }
      expect(response).to redirect_to(root_path)
    end
    it "deletes the user's session" do
      delete :destroy, params: {
        id: my_user.id
      }
      expect(assigns(:session)).to be_nil
    end
    it "flashes #notice" do
      delete :destroy, params: {
        id: my_user.id
      }
      expect(flash[:notice]).to be_present
    end
  end

end
[/sourcecode]

Then the implementation is fairly simple. We create a #create method and a #destroy method. In our create method, we search the database for a user with the specified email address in the params hash. Then we verify whether the user input password matches the database password. If the user is authenticated successfully, then we call create a session id (sign the user in) and redirect to home. If unsuccessful, we flash a warning message and allow them to try again. In our destroy method, we simply convert session id to nil (sign the user out), notify user they've been signed out, and redirect to home.

[sourcecode language="ruby" wraplines="true" collapse="true"]
module SessionsHelper
  def create_session(user)
    session[:user_id] = user.id
  end
  def destroy_session(user)
    session[:user_id] = nil
  end
  def current_user
    User.find_by(id: session[:user_id])
  end
end

class SessionsController *Comment Model*
In order to bind comments to posts, we must first create the Comment model and define the relationship. We define the Comment model as belonging to both the user that posted it and the post it was created on. Then of course, we go through the usual validations of comment body presence and length and user presence.

[sourcecode language="ruby" wraplines="true" collapse="true"]
class Comment  *Comments Controller*
Then, for the Comments controller, I create tests for #create and #destroy actions and write methods to pass these tests. My create method passes in comment params to the model to save the comment. My destroy method locates the proper comment and destroys it. Users are authorized based on whether they are the author (current_user == comment.user) or an admin.

[sourcecode language="ruby" wraplines="true" collapse="true"]
class CommentsController  *Comments View*
Finally, I display the comments using rendering a comment partial on the post page.

[sourcecode language="ruby" wraplines="true" collapse="true"]

 commented ago | 
```
[/sourcecode]

**6. Users can up/down vote any post.** *Vote Model* As with the Comment model, we define that vote model class belongs to the user and post model classes. Then we run a callback to ensure that we update the vote count on the post when someone upvotes the post.

[sourcecode language="ruby" wraplines="true" collapse="true"]
class Vote  
```
[/sourcecode]

**7. Users can favorite any post and be emailed with updates on that post.**  *Favorites View* Users can easily favorite or unfavorite a post (create or delete class). * ![Admin View of Favorite'd Post](https://nkimberly.wordpress.com/wp-content/uploads/2017/10/screenshot-from-2017-10-07-17-50-09-e1507426219286.png?w=2048)

[sourcecode language="ruby" wraplines="true" collapse="true"]

    * Unfavorite

    ** Favorite

[/sourcecode]

*Favorites Mailer* Here I've created a mailer to notify users who have favorited a post of any new comments. Of course, in production mode, you would want to use SendGrid and switch from localhost:3000 to the actual site you are deploying to. I also have it so that if a user creates a post, they automatically favorite that post and are notified of ensuing comments as well. ![Kimberly-comment-notification](https://nkimberly.wordpress.com/wp-content/uploads/2017/10/kimberly-comment-notification-e1507426519952.png)

[sourcecode language="ruby" wraplines="true" collapse="true"]
class FavoriteMailer 

##  
 , 

## Posts
## has not submitted any posts yet.

## Favorites
## has not favorited any posts yet.

## Comments
## has not submitted any comments yet.

[/sourcecode]

**9. Development database is seeded automatically with users, topics and posts.**
 *Seeds.rb*
And now for my favorite part! Seeding the database :) The code is rather self-explanatory... Essentially we write a couple of loops that randomly generate topics, posts, users, and vote values. We also make sure to use users.sample or topics.sample to create posts, votes, and comments.

[sourcecode language="ruby" wraplines="true" collapse="true"]
# This file should contain all the record creation needed to seed the database with its default values.
# The data can then be loaded with the rails db:seed command (or created alongside the database with db:setup).
#
# Examples:
#
#   movies = Movie.create([{ name: 'Star Wars' }, { name: 'Lord of the Rings' }])
#   Character.create(name: 'Luke', movie: movies.first)

require 'random_data'

5.times do
  User.create!(
    name: RandomData.random_name,
    email: RandomData.random_email,
    password: RandomData.random_sentence
  )
end

users = User.all

admin = User.create!(
  name:     'Admin User',
  email:    'admin@example.com',
  password: 'helloworld',
  role:     'admin'
)

mod = User.create!(
  name:     'Mod User',
  email:    'mod@example.com',
  password: 'helloworld',
  role:     'mod'
)

member = User.create!(
  name:     'Member User',
  email:    'member@example.com',
  password: 'helloworld'
)

5.times do |i|
  i = i.to_s
  Topic.create!(
    name: 'Topic ' + i,
    description: RandomData.random_paragraph
  )
end

topics = Topic.all

50.times do |i|
  i = i.to_s
  post = Post.create!(
    topic:  topics.sample,
    title: 'Post ' + i,
    body: RandomData.random_paragraph,
    user: users.sample
  )
  post.update_attribute(:created_at, rand(10.minutes .. 1.year).ago)

  rand(1..5).times {
    post.votes.create!(
      value: [-1, 1].sample,
      user: users.sample
    )
  }
end

posts = Post.all

100.times do |i|
  i = i.to_s
  Comment.create!(
    user: users.sample,
    post: posts.sample,
    body: 'Comment ' + i + ' : ' + RandomData.random_paragraph
  )
end

Post.find_or_create_by(title: "Unique Title", body: "Unique Body")

puts "Seed finished"
puts "#{User.count} users created"
puts "#{Topic.count} topics created"
puts "#{Post.count} posts created"
puts "#{Comment.count} comments created"
puts "#{Vote.count} votes created"
[/sourcecode]

Running the seed should output the following in your terminal,

[sourcecode language="ruby" wraplines="true" collapse="false"]
➜  bloccit git:(master) ✗ rails db:reset
Dropped database 'db/development.sqlite3'
Dropped database 'db/test.sqlite3'
Created database 'db/development.sqlite3'
Created database 'db/test.sqlite3'
-- create_table("advertisements", {:force=>:cascade})
   -> 0.0162s
-- create_table("answers", {:force=>:cascade})
   -> 0.0592s
-- create_table("comments", {:force=>:cascade})
   -> 0.0641s
-- create_table("favorites", {:force=>:cascade})
   -> 0.0751s
-- create_table("posts", {:force=>:cascade})
   -> 0.0672s
-- create_table("questions", {:force=>:cascade})
   -> 0.0195s
-- create_table("topics", {:force=>:cascade})
   -> 0.0214s
-- create_table("users", {:force=>:cascade})
   -> 0.0243s
-- create_table("votes", {:force=>:cascade})
   -> 0.0671s
-- create_table("advertisements", {:force=>:cascade})
   -> 0.0162s
-- create_table("answers", {:force=>:cascade})
   -> 0.0401s
-- create_table("comments", {:force=>:cascade})
   -> 0.0497s
-- create_table("favorites", {:force=>:cascade})
   -> 0.0542s
-- create_table("posts", {:force=>:cascade})
   -> 0.0520s
-- create_table("questions", {:force=>:cascade})
   -> 0.0154s
-- create_table("topics", {:force=>:cascade})
   -> 0.0185s
-- create_table("users", {:force=>:cascade})
   -> 0.0207s
-- create_table("votes", {:force=>:cascade})
   -> 0.0519s
Seed finished
8 users created
5 topics created
50 posts created
100 comments created
219 votes created
[/sourcecode]
### Solution
View demo at [https://biz-crud.herokuapp.com/](https://biz-crud.herokuapp.com/).

https://www.youtube.com/watch?v=Vkp-G4rQejA

Here's a view of the site on mobile.

![Post](https://nkimberly.wordpress.com/wp-content/uploads/2017/10/post-e1507428036445.png)

The application was written by Kimberly with guidance from the Bloc Web Developer curriculum and Bloc Mentor Ben Neely.