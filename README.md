# python-saml-flask

python-saml-flask is an abstraction of [python-saml](https://github.com/onelogin/python-saml), to make for quick integration into flask apps (or django with a bit of work).

It is not an official extension, but acts just like one.


## Setup

1. Save saml.py into your app somewhere (in this example its in a folder called lib)
2. Install `flask-login` and `flask-saml`
3. Add instantiation code:

    ```
    #import
    from flask.ext.login import LoginManager
    from lib.saml import SamlManager

    #setup flask-login
    login_manager = LoginManager()
    login_manager.init_app(app)
    login_manager.login_view = '/saml/login'

    #setup saml manager
    saml_manager = SamlManager()
    saml_manager.init_app(app)
    @saml_manager.login_from_acs
    def acs_login(acs):
      # define login logic here depending on idp response
      # should call login_user() and redirect as necessary
      pass
    ```

4. Add settings and certs:

    * `SAML_SETTINGS_PATH` - String - Optional
      * otherwise renders `saml_logout_successful.html` template
    * `SAML_LOGOUT_PATH` - STRING, - Required
      * path to a folder named saml which has:
        * settings.json
        * advanced_settings.json
        * a folder named 'certs' with all certs/keys

5. Create Metadata and trade with IDP

    ```
    from lib.saml import SamlRequest
    from flask import request
    @app.route('/saml/metadata')
    def metadata():
      saml = SamlRequest(request)
      return saml.generate_metadata()
    ```

6. Define `acs_login`. The data you get back will depend on your IDP, so throw a debugger in there to see what was passed in.
7. It should now work, you'll have to communicate with your IDP to finalize everything.


## Contributing

Contributions are welocme! Please follow these steps:

1. Fork this repository
2. Make your changes
3. Submit PR

