### 3.3.0 (unreleased)

* enhancements
  * Add the `expire_all_remember_me_on_sign_out` configuration to invalidate
    all the remember me tokens when the user signs out. (by @laurocaetano)
  * Default email messages was updated with grammar fixes, check the diff on
    #2906 for the updated copy (by @p-originate)
  * Allow a resource to be found based on its encrypted password token (by @karlentwistle)

* bug fix
  * `SessionsController#destroy` no longer yields the `resource` to receiving block,
    since the resource isn't loaded in the action. If you need access to the current
    resource when overring the action use the scope helper (like `current_user`) before
    calling `super`
  * Serialize the `last_request_at` entry as an Integer
  * Ensure registration controller block yields happen on failure in addition to success (by @dpehrson)

### 3.2.4

* enhancements
  * `bcrypt` dependency updated due https://github.com/codahale/bcrypt-ruby/pull/86.
  * View generator now can generate specific views with the `-v` flag, like `rails g devise:views -v sessions` (by @kayline)

### 3.2.3

* enhancements
  * Devise will use the `secret_key_base` on Rails 4+ applications as its `secret_key`.
    You can change this and use your own secret by changing the `devise.rb` initializer.

* bug fix
  * Migrations will be properly generated when using rails 4.1.0.

### 3.2.2

* bug fix
  * Ensure timeoutable works when `sign_out_all_scopes` is false (by @louman)
  * Keep the query string when storing location (by @csexton)
  * Require rails generator base class in devise generators

### 3.2.1

Security announcement: http://blog.plataformatec.com.br/2013/11/e-mail-enumeration-in-devise-in-paranoid-mode

* enhancements
  * Add `store_location_for` helper and ensure it is safe (by @matthewrudy and @homakov)
  * Add `yield` around resource methods in Devise controllers (by @edelpero)

* bug fix
  * Bring `password_digest` back to fix compatibility with `devise-encryptable`
  * Avoid e-mail enumeration on sign in when in paranoid mode

### 3.2.0

* enhancements
  * Previously deprecated token authenticatable and insecure lookups have been removed
  * Add a class method so you can encrypt passwords from fixtures (by @tenderlove)
  * Send custom message when user enters invalid password and it has only one attempt
  to enter correct password before their account will be locked (by @Lightpower)
  * Prevent mutation of values assigned to case and whitespace santitized members (by @iamvery)
  * Separate redirects and flash messages in `navigational_formats` and `flashing_formats` (by @ssendev)

* bug fix
  * A GET to sign_in page shouldn't extend the session (by @drewish)
  * Splat the arguments to `strong_parameters#permit` to work around a limitation in the `strong_parameters` gem (by @memberful)
  * Omniauth now uses `mapping.fullpath` when generating routes. This means if you call `devise_for :users` inside a scope, like `scope "/api"`, the scope will now apply to the omniauth route (by @AlexanderZaytsev)
  * Ensure timeoutable hook respects `Devise.sign_out_all_scopes` configuration

* deprecations
  * `expire_session_data_after_sign_in!` has been deprecated in favor of `expire_data_after_sign_in!`

### 3.1.1

* bug fix
  * Improve default message which asked users to sign in even when they were already signed (by @gregates)
  * Improve error message for when the config.secret_key is missing

### 3.1.0

Security announcement: http://blog.plataformatec.com.br/2013/08/devise-3-1-now-with-more-secure-defaults/

* backwards incompatible changes
  * Do not store confirmation, unlock and reset password tokens directly in the database. This means tokens previously stored in the database are no longer valid. You can reenable this temporarily by setting `config.allow_insecure_token_lookup = true` in your configuration file. It is recommended to keep this configuration set to true just temporarily in your production servers only to aid migration
  * The Devise mailer and its views were changed to explicitly receive a token argument as `@token`. You will need to update your mailers and re-copy the views to your application with `rails g devise:views`
  * Sanitization of parameters should be done by calling `devise_parameter_sanitizer.sanitize(:action)` instead of `devise_parameter_sanitizer.for(:action)`

* deprecations
  * Token authentication is deprecated

* enhancements
  * Better security defaults
  * Allow easier customization of parameter sanitizer (by @alexpeattie)

* bug fix
  * Do not confirm e-mail after password reset (by @moll)
  * Do not sign in after confirmation
  * Do not store confirmation, unlock and reset password tokens directly in the database
  * Do not compare directly against confirmation, unlock and reset password tokens
  * Skip storage for cookies on unverified requests

### 3.0.2

* bug fix
  * Skip storage for cookies on unverified requests

### 3.0.1

Security announcement: http://blog.plataformatec.com.br/2013/08/csrf-token-fixation-attacks-in-devise/

* enhancements
  * Add after_confirmation callback

* bug fix
  * When using rails 3.2, the generator adds 'attr_accessible' to the model (by @jcoyne)
  * Clean up CSRF token after authentication (by @homakov). Notice this change will clean up the CSRF Token after authentication (sign in, sign up, etc). So if you are using AJAX for such features, you will need to fetch a new CSRF token from the server.

### 3.0.0

* enhancements
  * Rails 4 and Strong Parameters compatibility (by @carlosantoniodasilva, @josevalim, @latortuga, @lucasmazza, @nashby, @rafaelfranca, @spastorino)
  * Drop support for Rails < 3.2 and Ruby < 1.9.3
  * Enable to skip sending reconfirmation email when reconfirmable is on and `skip_confirmation_notification!` is invoked (by @tkhr)

* bug fix
  * Errors on unlock are now properly reflected on the first `unlock_keys`

### 2.2.4

* enhancements
  * Add `destroy_with_password` to `DatabaseAuthenticatable`. Allows destroying a record when `:current_password` matches, similarly to how `update_with_password` works. (by @michiel3)
  * Allow to override path after password resetting (by @worker8)
  * Add `#skip_confirmation_notification!` method to `Confirmable`. Allows skipping confirmation email without auto-confirming. (by @gregates)
  * allow_unconfirmed_access_for config from `:confirmable` module can be set to `nil` that means unconfirmed access for unlimited time. (by @nashby)
  * Support Rails' token strategy on authentication (by @robhurring)
  * Support explicitly setting the http authentication key via `config.http_authentication_key` (by @neo)

* bug fix
  * Do not redirect when accessing devise API via JSON. (by @sebastianwr)
  * Generating scoped devise views now uses the correct scoped shared links partial instead of the default devise one (by @nashby)
  * Fix inheriting mailer templates from `Devise::Mailer`
  * Fix a bug when procs are used as default mailer in Devise (by @tomasv)

* backwards incompatible changes
  * Changes on session storage will expire all existing sessions on upgrade. For those storing the session in the DB, they can be upgraded according to this gist: https://gist.github.com/moll/6417606

### 2.2.3

Security announcement: http://blog.plataformatec.com.br/2013/01/security-announcement-devise-v2-2-3-v2-1-3-v2-0-5-and-v1-5-3-released/

* bug fix
  * Require string conversion for all values

### 2.2.2

* bug fix
  * Fix bug when checking for reconfirmable in templates

### 2.2.1

* bug fix
  * Fix regression with case_insensitive_keys
  * Fix regression when password is blank when it is invalid

### 2.2.0

* backwards incompatible changes
  * `headers_for` is deprecated, customize the mailer directly instead
  * All mailer methods now expect a second argument with delivery options
  * Default minimum password length is now 8 (by @carlosgaldino)
  * Support alternate sign in error message when email record does not exist (this adds a new I18n key to the locale file) (by @gabetax)
  * DeviseController responds only to HTML requests by default (call `DeviseController.respond_to` or `ApplicationController.respond_to` to add new formats)
  * Support Mongoid 3 onwards (by @durran)

* enhancements
  * Fix unlockable which could leak account existence on paranoid mode (by @latortuga)
  * Confirmable now has a confirm_within option to set a period while the confirmation token is still valid (by @promisedlandt)
  * Flash messages in controller now respects `resource_name` (by @latortuga)
  * Separate `sign_in` and `sign_up` on RegistrationsController (by @rubynortheast)
  * Add autofocus to default views (by @Radagaisus)
  * Unlock user on password reset (by @marcinb)
  * Allow validation callbacks to apply to virtual attributes (by @latortuga)

* bug fix
  * unconfirmed_email now uses the proper e-mail on salutation
  * Fix default email_regexp config to not allow spaces (by @kukula)
  * Fix a regression introduced on warden 1.2.1 (by @ejfinneran)
  * Properly camelize omniauth strategies (by @saizai)
  * Do not set flash messages for non navigational requests on session sign out (by @mathieul)
  * Set the proper fields as required on the lockable module (by @nickhoffman)
  * Respects Devise mailer default's reply_to (by @mrchrisadams)
  * Properly assign resource on `sign_in` related action (by @adammcnamara)
  * `update_with_password` doesn't change encrypted password when it is invalid (by @nashby)
  * Properly handle namespaced models on Active Record generator (by @nashby)

### 2.1.4

* bugfix
  * Do not confirm account after reset password

### 2.1.3

* bugfix
  * Require string conversion for all values

### 2.1.2

* enhancements
  * Handle backwards incompatibility between Rails 3.2.6 and Thor 0.15.x

* bug fix
  * Fix regression on strategy validation on previous release

### 2.1.1 (yanked)

* enhancements
  * `sign_out_all_scopes` now locks warden and does not allow new logins in the same action
  * `Devise.omniauth_path_prefix` is available to configure omniauth path prefix
  * Redirect to sign in page when trying to access password#edit without a token (by @gbataille)
  * Allow a lambda in authenticate(d) routes helpers to further select the scope
  * Removed warnings on Rails 3.2.6 (by @nashby)

* bug fix
  * `update_with_password` now relies on assign_attributes and forwards the :as option (by @wtn)
  * Do not trigger timeout on sign in related actions
  * Timeout does not explode when reset_authentication_token! is accidentally defined by Active Model (by @remomueller)

* deprecations
  * Strategy#validate() no longer validates nil resources

### 2.1.0

* enhancements
  * Add `check_fields!(model_class)` method on Devise::Models to check if the model includes the fields that Devise uses
  * Add `skip_reconfirmation!` to skip reconfirmation
  * Devise model generator now works with engines
  * Devise encryptable was moved to its new gem (http://github.com/plataformatec/devise-encryptable)

* deprecations
  * Deprecations warnings added on Devise 2.0 are now removed with their features
  * All devise modules should now have a `required_fields(klass)` module method to help gathering missing attributes
  * `use_salt_as_remember_token` and `apply_schema` does not have any effect since 2.0 and are now deprecated
  * `valid_for_authentication?` must now return a boolean

* bug fix
  * Ensure after sign in hook is not called without a resource
  * Fix a term: now on Omniauth related flash messages, we say that we're authenticating from an omniauth provider instead of authorizing
  * Fixed redirect when authenticated mounted apps (by @hakanensari)
  * Ensure the failure app still respects config.relative_url_root
  * `/users/sign_in` doesn't choke on protected attributes used to select sign in scope (by @Paymium)
  * `failed_attempts` is set to zero after any sign in (including via reset password) (by @rodrigoflores)
  * Added token expiration on timeout (by @antiarchitect)
  * Do not accidentally mark `_prefixes` as private
  * Better support for custom strategies on test helpers (by @mattconnolly)
  * Return `head :no_content` in SessionsController now that most JS libraries handle it (by @julianvargasalvarez)
  * Reverted moving devise/shared/_links.erb to devise/_links.erb

### 2.0.4

Notes: https://github.com/plataformatec/devise/wiki/How-To:-Upgrade-to-Devise-2.0

* bug fix
  * Fix when :host is used with devise_for  (by @mreinsch)
  * Fix a regression that caused Warden to be initialized too late

### 2.0.3 (yanked)

* bug fix
  * Ensure warning is not shown by mistake on apps with mounted engines
  * Fixes related to remember_token and rememberable_options
  * Ensure serializable_hash does not depend on accessible attributes
  * Ensure that timeout callback does not run on sign out action

### 2.0.2

* enhancements
  * Add devise_i18n_options to customize I18n message

* bug fix
  * Ensure Devise.available_router_name defaults to :main_app
  * Set autocomplete to off for password on edit forms
  * Better error messages in case a trackable model can't be saved
  * Show a warning in case someone gives a pluralized name to devise generator
  * Fix test behavior for rspec subject requests (by @sj26)

### 2.0.1

* enhancements
  * Improved error messages on deprecation warnings
  * Hide Devise's internal generators from `rails g` command

* bug fix
  * Removed tmp and log files from gem

### 2.0.0

* enhancements
  * Add support for e-mail reconfirmation on change (by @Mandaryn and @heimidal)
  * Redirect users to sign in page after unlock (by @nashby)
  * Redirect to the previous URL on timeout
  * Inherit from the same Devise parent controller (by @sj26)
  * Allow parent_controller to be customizable via Devise.parent_controller, useful for engines
  * Allow router_name to be customizable via Devise.router_name, useful for engines
  * Allow alternate ORMs to run compatibility setup code before Authenticatable is included (by @jm81)

* deprecation
  * Devise now only supports Rails 3.1 forward
  * Devise.confirm_within was deprecated in favor Devise.allow_unconfirmed_access_for
  * Devise.stateless_token= is deprecated in favor of appending :token_auth to Devise.skip_session_storage
  * Usage of Devise.apply_schema is deprecated
  * Usage of Devise migration helpers are deprecated
  * Usage of Devise.remember_across_browsers was deprecated
  * Usage of rememberable with remember_token was removed
  * Usage of recoverable without reset_password_sent_at was removed
  * Usage of Devise.case_insensitive_keys equals to false was removed
  * Move devise/shared/_links.erb to devise/_links.erb
  * Deprecated support of nested devise_for blocks
  * Deprecated support to devise.registrations.reasons and devise.registrations.inactive_signed_up in favor of devise.registrations.signed_up_but_*
  * Protected method render_with_scope was removed.

### 1.5.3

* bug fix
  * Ensure delegator converts scope to symbol (by @dmitriy-kiriyenko)
  * Ensure passing :format => false to devise_for is not permanent
  * Ensure path checker does not check invalid routes

### 1.5.2

* enhancements
  * Add support for Rails 3.1 new mass assignment conventions (by @kirs)
  * Add timeout_in method to Timeoutable, it can be overridden in a model (by @lest)

* bug fix
  * OmniAuth error message now shows the proper option (:strategy_class instead of :klass)

### 1.5.1

* bug fix
  * Devise should not attempt to load OmniAuth strategies. Strategies should be loaded before hand by the developer or explicitly given to Devise.

### 1.5.0

* enhancements
  * Timeoutable also skips tracking if skip_trackable is given
  * devise_for now accepts :failure_app as an option
  * Models can select the proper mailer via devise_mailer method (by @locomotivecms)
  * Migration generator now uses the change method (by @nashby)
  * Support to markerb templates on the mailer generator (by @sbounmy)
  * Support for Omniauth 1.0 (older versions are no longer supported) (by @TamiasSibiricus)

* bug fix
  * Allow idempotent API requests
  * Fix bug where logs did not show 401 as status code
  * Change paranoid settings to behave as success instead of as failure
  * Fix bug where activation messages were shown first than the credentials error message
  * Instance variables are expired after sign out

* deprecation
  * redirect_location is deprecated, please use after_sign_in_path_for
  * after_sign_in_path_for now redirects to session[scope_return_to] if any value is stored in it

### 1.4.9

* bug fix
  * url helpers were not being set under some circumstances

### 1.4.8

* enhancements
  * Add docs for assets pipeline and Heroku

* bug fix
  * confirmation_url was not being set under some circumstances

### 1.4.7

* bug fix
  * Fix backward incompatible change from 1.4.6 for those using custom controllers

### 1.4.6 (yanked)

* enhancements
  * Allow devise_for :skip => :all
  * Allow options to be passed to authenticate_user!
  * Allow --skip-routes to devise generator
  * Add allow_params_authentication! to make it explicit when params authentication is allowed in a controller

### 1.4.5

* bug fix
  * Failure app tries the root path if a session one does not exist
  * No need to finalize Devise helpers all the time (by @bradleypriest)
  * Reset password shows proper message if user is not active
  * `clean_up_passwords` sets the accessors to nil to skip validations

### 1.4.4

* bug fix
  * Do not always skip helpers, instead provide :skip_helpers as option to trigger it manually

### 1.4.3

* enhancements
  * Improve Rails 3.1 compatibility
  * Use serialize_into_session and serialize_from_session in Warden serialize to improve extensibility

* bug fix
  * Generator properly generates a change_table migration if a model already exists
  * Properly deprecate setup_mail
  * Fix encoding issues with email regexp
  * Only generate helpers for the used mappings
  * Wrap :action constraints in the proper hash

* deprecations
  * Loosened the used email regexp to simply assert the existent of "@". If someone relies on a more strict regexp, they may use https://github.com/SixArm/sixarm_ruby_email_address_validation

### 1.4.2

* bug fix
  * Provide a more robust behavior to serializers and add :force_except option

### 1.4.1

* enhancements
  * Add :defaults and :format support on router
  * Add simple form generators
  * Better localization for devise_error_messages! (by @zedtux)

* bug fix
  * Ensure to_xml is properly white listened
  * Ensure handle_unverified_request clean up any cached signed-in user

### 1.4.0

* enhancements
  * Added authenticated and unauthenticated to the router to route the used based on their status (by @sj26)
  * Improve e-mail regexp (by @rodrigoflores)
  * Add strip_whitespace_keys and default to e-mail (by @swrobel)
  * Do not run format and uniqueness validations on e-mail if it hasn't changed (by @Thibaut)
  * Added update_without_password to update models but not allowing the password to change (by @fschwahn)
  * Added config.paranoid, check the generator for more information (by @rodrigoflores)

* bug fix
  * password_required? should not affect length validation
  * User cannot access sign up and similar pages if they are already signed in through a cookie or token
  * Do not convert booleans to strings on finders (by @xavier)
  * Run validations even if current_password fails (by @crx)
  * Devise now honors routes constraints (by @macmartine)
  * Do not return the user resource when requesting instructions (by @rodrigoflores)

### 1.3.4

* bug fix
  * Do not add formats if html or "*/*"

### 1.3.3

* bug fix
  * Explicitly mark the token as expired if so

### 1.3.2

* bug fix
  * Fix another regression related to reset_password_sent_at (by @alexdreher)

### 1.3.1

* enhancements
  * Improve failure_app responses (by @indirect)
  * sessions/new and registrations/new also respond to xml and json now

* bug fix
  * Fix a regression that occurred if reset_password_sent_at is not present (by @stevehodgkiss)

### 1.3.0

* enhancements
  * All controllers can now handle different mime types than html using Responders (by @sikachu)
  * Added reset_password_within as configuration option to send the token for recovery (by @jdguyot)
  * Bump password length to 128 characters (by @k33l0r)
  * Add :only as option to devise_for (by @timoschilling)
  * Allow to override path after sending password instructions (by @irohiroki)
  * require_no_authentication has its own flash message (by @jackdempsey)

* bug fix
  * Fix a bug where configuration options were being included too late
  * Ensure Devise::TestHelpers can be used to tests Devise internal controllers (by @jwilger)
  * valid_password? should not choke on empty passwords (by @mikel)
  * Calling devise more than once does not include previously added modules anymore
  * downcase_keys before validation

* backward incompatible changes
  * authentication_keys are no longer considered when creating the e-mail validations, the previous behavior was buggy. You must double check if you were relying on such behavior.

### 1.2.1

* enhancements
  * Improve update path messages

### 1.2.0

* bug fix
  * Properly ignore path prefix on omniauthable
  * Faster uniqueness queries
  * Rename active? to active_for_authentication? to avoid conflicts

### 1.2.rc2

* enhancements
  * Make friendly_token 20 chars long
  * Use secure_compare

* bug fix
  * Fix an issue causing infinite redirects in production
  * rails g destroy works properly with devise generators (by @andmej)
  * before_failure callbacks should work on test helpers (by @twinge)
  * rememberable cookie now is httponly by default (by @JamesFerguson)
  * Add missing confirmation_keys (by @JohnPlummer)
  * Ensure after_* hooks are called on RegistrationsController
  * When using database_authenticatable Devise will now only create an email field when appropriate (if using default authentication_keys or custom authentication_keys with email included)
  * Ensure stateless token does not trigger timeout (by @pixelauthority)
  * Implement handle_unverified_request for Rails 3.0.4 compatibility and improve FailureApp reliance on symbols
  * Consider namespaces while generating routes
  * Custom failure apps no longer ignored in test mode (by @jaghion)
  * Do not depend on ActiveModel::Dirty
  * Manual sign_in now triggers remember token
  * Be sure to halt strategies on failures
  * Consider SCRIPT_NAME on Omniauth paths
  * Reset failed attempts when lock is expired
  * Ensure there is no Mongoid injection

* deprecations
  * Deprecated anybody_signed_in? in favor of signed_in? (by @gavinhughes)
  * Removed --haml and --slim view templates
  * Devise::OmniAuth helpers were deprecated and removed in favor of Omniauth.config.test_mode

### 1.2.rc

* deprecations
  * cookie_domain is deprecated in favor of cookie_options
  * after_update_path_for can no longer be defined in ApplicationController

* enhancements
  * Added OmniAuth support
  * Added ORM adapter to abstract ORM iteraction
  * sign_out_via is available in the router to configure the method used for sign out (by @martinrehfeld)
  * Improved Ajax requests handling in failure app (by @spastorino)
  * Added request_keys to easily use request specific values (like subdomain) in authentication
  * Increased the size of friendly_token to 60 characters (reduces the chances of a successful brute attack)
  * Ensure the friendly token does not include "_" or "-" since some e-mails may not autolink it properly (by @rymai)
  * Extracted encryptors into :encryptable for better bcrypt support
  * :rememberable is now able to use salt as token if no remember_token is provided
  * Store the salt in session and expire the session if the user changes their password
  * Allow :stateless_token to be set to true avoiding users to be stored in session through token authentication
  * cookie_options uses session_options values by default
  * Sign up now checks if the user is active or not and redirect them accordingly, setting the inactive_signed_up message
  * Use ActiveModel#to_key instead of #id
  * sign_out_all_scopes now destroys the whole session
  * Added case_insensitive_keys that automatically downcases the given keys, by default downcases only e-mail (by @adahl)

* default behavior changes
  * sign_out_all_scopes defaults to true as security measure
  * http authenticatable is disabled by default
  * Devise does not intercept 401 returned from applications

* bugfix
  * after_sign_in_path_for always receives a resource
  * Do not execute Warden::Callbacks on Devise::TestHelpers (by @sgronblo)
  * Allow password recovery and account unlocking to change used keys (by @RStankov)
  * FailureApp now properly handles nil request.format
  * Fix a bug causing FailureApp to return with HTTP Auth Headers for IE7
  * Ensure namespaces has proper scoped views
  * Ensure Devise does not set empty flash messages (by @sxross)

### 1.1.6

* Use a more secure e-mail regexp
* Implement Rails 3.0.4 handle unverified request
* Use secure_compare to compare passwords

### 1.1.5

* bugfix
  * Ensure to convert keys on indifferent hash

* defaults
  * Set config.http_authenticatable to false to avoid confusion

### 1.1.4

* bugfix
  * Avoid session fixation attacks

### 1.1.3

* bugfix
  * Add reply-to to e-mail headers by default
  * Updated the views generator to respect the rails :template_engine option (by @fredwu)
  * Check the type of HTTP Authentication before using Basic headers
  * Avoid invalid_salt errors by checking salt presence (by @thibaudgg)
  * Forget user deletes the right cookie before logout, not remembering the user anymore (by @emtrane)
  * Fix for failed first-ever logins on PostgreSQL where column default is nil (by @bensie)
  * :default options is now honored in migrations

### 1.1.2

* bugfix
  * Compatibility with latest Rails routes schema

### 1.1.1

* bugfix
  * Fix a small bug where generated locale file was empty on devise:install

### 1.1.0

* enhancements
  * Rememberable module allows user to be remembered across browsers and is enabled by default (by @trevorturk)
  * Rememberable module allows you to activate the period the remember me token is extended (by @trevorturk)
  * devise_for can now be used together with scope method in routes but with a few limitations (check the documentation)
  * Support `as` or `devise_scope` in the router to specify controller access scope
  * HTTP Basic Auth can now be disabled/enabled for xhr(ajax) requests using http_authenticatable_on_xhr option (by @pellja)

* bug fix
  * Fix a bug in Devise::TestHelpers where current_user was returning a Response object for non active accounts
  * Devise should respect script_name and path_info contracts
  * Fix a bug when accessing a path with (.:format) (by @klacointe)
  * Do not add unlock routes unless unlock strategy is email or both
  * Email should be case insensitive
  * Store classes as string in session, to avoid serialization and stale data issues

* deprecations
  * use_default_scope is deprecated and has no effect. Use :as or :devise_scope in the router instead

### 1.1.rc2

* enhancements
  * Allow to set cookie domain for the remember token. (by @mantas)
  * Added navigational formats to specify when it should return a 302 and when a 401.
  * Added authenticate(scope) support in routes (by @wildchild)
  * Added after_update_path_for to registrations controller (by @thedelchop)
  * Allow the mailer object to be replaced through config.mailer = "MyOwnMailer"

* bug fix
  * Fix a bug where session was timing out on sign out

* deprecations
  * bcrypt is now the default encryptor
  * devise.mailer.confirmations_instructions now should be devise.mailer.confirmations_instructions.subject
  * devise.mailer.user.confirmations_instructions now should be devise.mailer.confirmations_instructions.user_subject
  * Generators now use Rails 3 syntax (devise:install) instead of devise_install

### 1.1.rc1

* enhancements
  * Rails 3 compatibility
  * All controllers and views are namespaced, for example: Devise::SessionsController and "devise/sessions"
  * Devise.orm is deprecated. This reduces the required API to hook your ORM with devise
  * Use metal for failure app
  * HTML e-mails now have proper formatting
  * Allow to give :skip and :controllers in routes
  * Move trackable logic to the model
  * E-mails now use any template available in the filesystem. Easy to create multipart e-mails
  * E-mails asks headers_for in the model to set the proper headers
  * Allow to specify haml in devise_views
  * Compatibility with Mongoid
  * Make config.devise available on config/application.rb
  * TokenAuthenticatable now works with HTTP Basic Auth
  * Allow :unlock_strategy to be :none and add :lock_strategy which can be :failed_attempts or none. Setting those values to :none means that you want to handle lock and unlocking by yourself
  * No need to append ?unauthenticated=true in URLs anymore since Flash was moved to a middleware in Rails 3
  * :activatable is included by default in your models

* bug fix
  * Fix a bug with STI

* deprecations
  * Rails 3 compatible only
  * Removed support for MongoMapper
  * Scoped views are no longer "sessions/users/new". Now use "users/sessions/new"
  * Devise.orm is deprecated, just require "devise/orm/YOUR_ORM" instead
  * Devise.default_url_options is deprecated, just modify ApplicationController.default_url_options
  * All messages under devise.sessions, except :signed_in and :signed_out, should be moved to devise.failure
  * :as and :scope in routes is deprecated. Use :path and :singular instead

### 1.0.8

* enhancements
  * Support for latest MongoMapper
  * Added anybody_signed_in? helper (by @SSDany)

* bug fix
  * confirmation_required? is properly honored on active? calls. (by @paulrosania)

### 1.0.7

* bug fix
  * Ensure password confirmation is always required

* deprecations
  * authenticatable was deprecated and renamed to database_authenticatable
  * confirmable is not included by default on generation

### 1.0.6

* bug fix
  * Do not allow unlockable strategies based on time to access a controller.
  * Do not send unlockable email several times.
  * Allow controller to upstram custom! failures to Warden.

### 1.0.5

* bug fix
  * Use prepend_before_filter in require_no_authentication.
  * require_no_authentication on unlockable.
  * Fix a bug when giving an association proxy to devise.
  * Do not use lock! on lockable since it's part of ActiveRecord API.

### 1.0.4

* bug fix
  * Fixed a bug when deleting an account with rememberable
  * Fixed a bug with custom controllers

### 1.0.3

* enhancements
  * HTML e-mails now have proper formatting
  * Do not remove MongoMapper options in find

### 1.0.2

* enhancements
  * Allows you set mailer content type (by @glennr)

* bug fix
  * Uses the same content type as request on http authenticatable 401 responses

### 1.0.1

* enhancements
  * HttpAuthenticatable is not added by default automatically.
  * Avoid mass assignment error messages with current password.

* bug fix
  * Fixed encryptors autoload

### 1.0.0

* deprecation
  * :old_password in update_with_password is deprecated, use :current_password instead

* enhancements
  * Added Registerable
  * Added Http Basic Authentication support
  * Allow scoped_views to be customized per controller/mailer class
  * Allow authenticatable to used in change_table statements

### 0.9.2

* bug fix
  * Ensure inactive user cannot sign in
  * Ensure redirect to proper url after sign up

* enhancements
  * Added gemspec to repo
  * Added token authenticatable (by @grimen)

### 0.9.1

* bug fix
  * Allow bigger salt size (by @jgeiger)
  * Fix relative url root

### 0.9.0

* deprecation
  * devise :all is deprecated
  * :success and :failure flash messages are now :notice and :alert

* enhancements
  * Added devise lockable (by @mhfs)
  * Warden 0.9.0 compatibility
  * Mongomapper 0.6.10 compatibility
  * Added Devise.add_module as hooks for extensions (by @grimen)
  * Ruby 1.9.1 compatibility (by @grimen)

* bug fix
  * Accept path prefix not starting with slash
  * url helpers should rely on find_scope!

### 0.8.2

* enhancements
  * Allow Devise.mailer_sender to be a proc (by @grimen)

* bug fix
  * Fix bug with passenger, update is required to anyone deploying on passenger (by @dvdpalm)

### 0.8.1

* enhancements
  * Move salt to encryptors
  * Devise::Lockable
  * Moved view links into partial and I18n'ed them

* bug fix
  * Bcrypt generator was not being loaded neither setting the proper salt

### 0.8.0

* enhancements
  * Warden 0.8.0 compatibility
  * Add an easy for map.connect "sign_in", :controller => "sessions", :action => "new" to work
  * Added :bcrypt encryptor (by @capotej)

* bug fix
  * sign_in_count is also increased when user signs in via password change, confirmation, etc..
  * More DataMapper compatibility (by @lancecarlson)

* deprecation
  * Removed DeviseMailer.sender

### 0.7.5

* enhancements
  * Set a default value for mailer to avoid find_template issues
  * Add models configuration to MongoMapper::EmbeddedDocument as well

### 0.7.4

* enhancements
  * Extract Activatable from Confirmable
  * Decouple Serializers from Devise modules

### 0.7.3

* bug fix
  * Give scope to the proper model validation

* enhancements
  * Mail views are scoped as well
  * Added update_with_password for authenticatable
  * Allow render_with_scope to accept :controller option

### 0.7.2

* deprecation
  * Renamed reset_confirmation! to resend_confirmation!
  * Copying locale is part of the installation process

* bug fix
  * Fixed render_with_scope to work with all controllers
  * Allow sign in with two different users in Devise::TestHelpers

### 0.7.1

* enhancements
  * Small enhancements for other plugins compatibility (by @grimen)

### 0.7.0

* deprecations
  * :authenticatable is not included by default anymore

* enhancements
  * Improve loading process
  * Extract SessionSerializer from Authenticatable

### 0.6.3

* bug fix
  * Added trackable to migrations
  * Allow inflections to work

### 0.6.2

* enhancements
  * More DataMapper compatibility
  * Devise::Trackable - track sign in count, timestamps and ips

### 0.6.1

* enhancements
  * Devise::Timeoutable - timeout sessions without activity
  * DataMapper now accepts conditions

### 0.6.0

* deprecations
  * :authenticatable is still included by default, but yields a deprecation warning

* enhancements
  * Added DataMapper support
  * Remove store_location from authenticatable strategy and add it to failure app
  * Allow a strategy to be placed after authenticatable
  * Do not rely attribute? methods, since they are not added on Datamapper

### 0.5.6

* enhancements
  * Do not send nil to build (DataMapper compatibility)
  * Allow to have scoped views

### 0.5.5

* enhancements
  * Allow overwriting find for authentication method
  * Remove Ruby 1.8.7 dependency

### 0.5.4

* deprecations
  * Deprecate :singular in devise_for and use :scope instead

* enhancements
  * Create after_sign_in_path_for and after_sign_out_path_for hooks to be
    overwriten in ApplicationController
  * Create sign_in_and_redirect and sign_out_and_redirect helpers
  * Warden::Manager.default_scope is automatically configured to the first given scope

### 0.5.3

* bug fix
  * MongoMapper now converts DateTime to Time
  * Ensure all controllers are unloadable

* enhancements
  * Moved friendly_token to Devise
  * Added Devise.all, so you can freeze your app strategies
  * Added Devise.apply_schema, so you can turn it to false in Datamapper or MongoMapper
    in cases you don't want it be handlded automatically

### 0.5.2

* enhancements
  * Improved sign_in and sign_out helpers to accepts resources
  * Added stored_location_for as a helper
  * Added test helpers

### 0.5.1

* enhancements
  * Added serializers based on Warden ones
  * Allow authentication keys to be set

### 0.5.0

* bug fix
  * Fixed a bug where remember me module was not working properly

* enhancements
  * Moved encryption strategy into the Encryptors module to allow several algorithms (by @mhfs)
  * Implemented encryptors for Clearance, Authlogic and Restful-Authentication (by @mhfs)
  * Added support for MongoMapper (by @shingara)

### 0.4.3

* bug fix
  * Authentication just fails if user cannot be serialized from session, without raising errors;
  * Default configuration values should not overwrite user values;

### 0.4.2

* deprecations
  * Renamed mail_sender to mailer_sender

* enhancements
  * skip_before_filter added in Devise controllers
  * Use home_or_root_path on require_no_authentication as well
  * Added devise_controller?, useful to select or reject filters in ApplicationController
  * Allow :path_prefix to be given to devise_for
  * Allow default_url_options to be configured through devise (:path_prefix => "/:locale" is now supported)

### 0.4.1

* bug fix
  * Ensure options can be set even if models were not loaded

### 0.4.0

* deprecations
  * Notifier is deprecated, use DeviseMailer instead. Remember to rename
    app/views/notifier to app/views/devise_mailer and I18n key from
    devise.notifier to devise.mailer
  * :authenticable calls are deprecated, use :authenticatable instead

* enhancements
  * Allow devise to be more agnostic and do not require ActiveRecord to be loaded
  * Allow Warden::Manager to be configured through Devise
  * Created a generator which creates an initializer

### 0.3.0

* bug fix
  * Allow yml messages to be configured by not using engine locales

* deprecations
  * Renamed confirm_in to confirm_within
  * Do not send confirmation messages when user changes their e-mail
  * Renamed authenticable to authenticatable and added deprecation warnings

### 0.2.3

* enhancements
  * Ensure fail! works inside strategies
  * Make unauthenticated message (when you haven't signed in) different from invalid message

* bug fix
  * Do not redirect on invalid authenticate
  * Allow model configuration to be set to nil

### 0.2.2

* bug fix
  * Fix a bug when using customized resources

### 0.2.1

* refactor
  * Clean devise_views generator to use devise existing views

* enhancements
  * Create instance variables (like @user) for each devise controller
  * Use Devise::Controller::Helpers only internally

* bug fix
  * Fix a bug with Mongrel and Ruby 1.8.6

### 0.2.0

* enhancements
  * Allow option :null => true in authenticable migration
  * Remove attr_accessible calls from devise modules
  * Customizable time frame for rememberable with :remember_for config
  * Customizable time frame for confirmable with :confirm_in config
  * Generators for creating a resource and copy views

* optimize
  * Do not load hooks or strategies if they are not used

* bug fixes
  * Fixed requiring devise strategies

### 0.1.1

* bug fixes
  * Fixed requiring devise mapping

### 0.1.0

* Devise::Authenticable
* Devise::Confirmable
* Devise::Recoverable
* Devise::Validatable
* Devise::Migratable
* Devise::Rememberable

* SessionsController
* PasswordsController
* ConfirmationsController

* Create an example app
* devise :all, :except => :rememberable
* Use sign_in and sign_out in SessionsController

* Mailer subjects namespaced by model
* Allow stretches and pepper per model

* Store session[:return_to] in session
* Sign user in automatically after confirming or changing it's password
