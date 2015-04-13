# contactlist
Contact list

1.5.1 Ruby Heroku setup

Heroku uses the PostgreSQL database (pronounced “post-gres-cue-ell”, and often called “Postgres” for short), which means that we need to add the pg gem in the production environment to allow Rails to talk to Postgres:17

group :production do
  gem 'pg',             '0.17.1'
  gem 'rails_12factor', '0.0.2'
end
Note also the addition of the rails_12factor gem, which is used by Heroku to serve static assets such as images and stylesheets. The resulting Gemfile appears as in Listing 1.14.

Listing 1.14: A Gemfile with added gems.
source 'https://rubygems.org'

gem 'rails',        '4.2.0'
gem 'sass-rails',   '5.0.1'
gem 'uglifier',     '2.5.3'
gem 'coffee-rails', '4.1.0'
gem 'jquery-rails', '4.0.3'
gem 'turbolinks',   '2.3.0'
gem 'jbuilder',     '2.2.3'
gem 'sdoc',         '0.4.0', group: :doc

group :development, :test do
  gem 'sqlite3',     '1.3.9'
  gem 'byebug',      '3.4.0'
  gem 'web-console', '2.0.0.beta3'
  gem 'spring',      '1.1.3'
end

group :production do
  gem 'pg',             '0.17.1'
  gem 'rails_12factor', '0.0.2'
end
To prepare the system for deployment to production, we run bundle install with a special flag to prevent the local installation of any production gems (which in this case consists of pg and rails_12factor):

$ bundle install --without production
Because the only gems added in Listing 1.14 are restricted to a production environment, right now this command doesn’t actually install any additional local gems, but it’s needed to update Gemfile.lock with the pg and rails_12factor gems. We can commit the resulting change as follows:

$ git commit -a -m "Update Gemfile.lock for Heroku"
Next we have to create and configure a new Heroku account. The first step is to sign up for Heroku. Then check to see if your system already has the Heroku command-line client installed:

$ heroku version
Those using the cloud IDE should see the Heroku version number, indicating that the heroku CLI is available, but on other systems it may be necessary to install it using the Heroku Toolbelt.18

Once you’ve verified that the Heroku command-line interface is installed, use the heroku command to log in and add your SSH key:

$ heroku login
$ heroku keys:add
Finally, use the heroku create command to create a place on the Heroku servers for the sample app to live (Listing 1.15).

Listing 1.15: Creating a new application at Heroku.
$ heroku create
Creating damp-fortress-5769... done, stack is cedar
http://damp-fortress-5769.herokuapp.com/ | git@heroku.com:damp-fortress-5769.git
Git remote heroku added
The heroku command creates a new subdomain just for our application, available for immediate viewing. There’s nothing there yet, though, so let’s get busy deploying.

1.5.2 Heroku deployment, step one

To deploy the application, the first step is to use Git to push the master branch up to Heroku:

$ git push heroku master
(You may see some warning messages, which you should ignore for now. We’ll discuss them further in Section 7.5.)

1.5.3 Heroku deployment, step two

There is no step two! We’re already done. To see your newly deployed application, visit the address that you saw when you ran heroku create (i.e., Listing 1.15). (If you’re working on your local machine instead of the cloud IDE, you can also use heroku open.) The result appears in Figure 1.18. The page is identical to Figure 1.12, but now it’s running in a production environment on the live web.

images/figures/heroku_app_hello_world
Figure 1.18: The first Rails Tutorial application running on Heroku.
1.5.4 Heroku commands

There are many Heroku commands, and we’ll barely scratch the surface in this book. Let’s take a minute to show just one of them by renaming the application as follows:

$ heroku rename rails-tutorial-hello
Don’t use this name yourself; it’s already taken by me! In fact, you probably shouldn’t bother with this step right now; using the default address supplied by Heroku is fine. But if you do want to rename your application, you can arrange for it to be reasonably secure by using a random or obscure subdomain, such as the following:

hwpcbmze.herokuapp.com
seyjhflo.herokuapp.com
jhyicevg.herokuapp.com
With a random subdomain like this, someone could visit your site only if you gave them the address. (By the way, as a preview of Ruby’s compact awesomeness, here’s the code I used to generate the random subdomains:

('a'..'z').to_a.shuffle[0..7].join
Pretty sweet.)

In addition to supporting subdomains, Heroku also supports custom domains. (In fact, the Ruby on Rails Tutorial site lives at Heroku; if you’re reading this book online, you’re looking at a Heroku-hosted site right now!) See the Heroku documentation for more information about custom domains and other Heroku topics.


