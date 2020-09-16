.. _workflow

Adding route
============

Adding a route is done in 3 steps.

Handlers
--------

Create or update an existing handler with a function with the query logic.

These should include responses: success and error handling.

Error responses can be found in __controllers/v1/errors__.

**Example:**

    *def get_current_user():
        username = get_jwt_identity()
        if isinstance(username, str):
            user = User.query.filter_by(username=username).first()
            return  {
                'user_id': user.id,
                'username': user.username,
                'email': user.email,
                'first_name': user.first_name,
                'last_name': user.last_name,
                'registered_on': user.registered_on
            }
    
        return errors.unauthorized(message='login_required')*


Controllers
-----------

In the appropriate controller, add a function with associated logic and/or handler.
        

Route config
------------

Define the route parameters.

- rule: Route url
- method: Route method
- endpoint: File in the controllers
- function: Function in the controller
- opts: 
    - use_pagination: Will paginate items in request
    - auth_required: Check if user needs to be logged in
    - auth_roles: Check if user has needed role

**Example:**

    *{
        'rule': '/users/me',
        'method': 'GET',
        'endpoint': 'users',
        'function': 'get_current',
        'opts': {
            'use_pagination': True,
            'auth_required': True,
            'auth_roles': ['ADMIN', 'ANNOTATOR']
        }
    },*
