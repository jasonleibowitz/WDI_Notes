* omniauth.rb
	* provider info
	
		```
		Rails.application.config.middleware.use OmniAuth::Builder do
  		provider :facebook, ENV['FACEBOOK_KEY'], ENV['FACEBOOK_SECRET'],
           :scope => 'email,user_birthday,read_stream', :display => 'popup'
		end
		```
* auth path
	* ```localhost:3000/auth/facebook```
* callback route
	* ```auth/facebook/callback```
* Create authentication scaffold
	* Authentication
		* attributes
			* user_id (integer)
			* provider (string) -- facebook
			* uid (string) - provider's user id
		* controller
			* index
			* create
			* destroy
* Update User Model
	* ```has many :authentications```
* Authentication Model
	
	```
	class Authentication < ActiveRecord::Base
		belongs_to :user
	end
	```
* Update routes
	
	```
	match '/auth/:provider/callback' => 'authentications#create'
	```
* Update Authentication Controller Create

	```
	def create
		auth = request.env["omniauth.auth"]
		current_user.authentications.create(provider: auth['provider'], uid: auth['hid'])
		flash[:notice] = 'Authentication successful'
		redirect_to authentications_url
	end
	```
* Update Authentication Index so that it only returns current user's authentications if there is a current user

	```
	def index
		@authentications = current_user.authentications if current_user
	end
	```
* Update Authentication Destroy so it goes through current user model

	```
	def destroy
		@authentication = current_user.authentications.find(params[:id])
		@authentication.destroy
		flash[:notice] = 'Successfully destroy authentication'
		redirect_to authentications_url
	end
	```
	
* Prevent user from authenticating twice at the same time
	* Go to authentications controller and update create call to ```find_or_create_by```
	
		```
		def create
			auth = request.env["omniauth.auth"]
			current_user.authentications.find_or_create_by_provider_and_uid(auth['provider'], uid: auth['uid'])
		end
		```