# SM Framework PostgreSQL Extension Set repmgr Extension

For information about the repmgr tool visit the website at http://repmgr.org/

## Dependencies

Before installing repmgr be sure that that PostgreSQL is installed via
the SM Framework.

The next requirements are libxslt and pam with development libraries.
On a RH based distribution this would be done via:

    root# sm postgresql install
    root# yum install -y libxslt libxslt-devel pam pam-devel

Once the dependencies are satisfied you may then install repmgr.

    root# sm repmgr install
    root# sm repmgr configure

Adjust any values you need to in the repmgr configuration file:

    /etc/postgresql/repmgr.conf

Now you may start using repmgr.

For detailed repmgr usage information postinstall view the repmgr project README
file:

    https://github.com/greg2ndQuadrant/repmgr

