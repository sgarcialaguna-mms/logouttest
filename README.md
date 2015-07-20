# logouttest
Demonstrate a possible bug in Django.

To reproduce:

Make sure your browser isn't set to disable cache (e.g. when Chrome DevTools are open)
Log in with admin/admin.
Log out.
Log in again.
Log out again.
=> Homepage still says "You are logged in as admin"

Note that I enabled caching for the logout page within Django, in production, the caching is configured within Apache.

This bug did not occur with Django 1.6. I believe the bug was introduced with commit https://github.com/django/django/commit/393c0e24223c701edeb8ce7dc9d0f852f0c081ad,
as previously Vary: Cookie header was set and so the logout page wouldn't be cached.

Additionally, the call to delete_cookie() should set the cookie path in case settings.COOKIE_PATH is not '/'.
