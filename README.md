Quickstart Explanation
----------------------

The following is an explanation of how this Quickstart was created so you can use it as a guide in creating your own Quickstarts.

* Config File: Because Pagoda Box needs a different config file than a local version of the site, we created a new directory in the root of the project called "pagoda" and created a pagoda version of the config file there. Then we created an After Build deploy hook in the Boxfile that moved that file from pagoda/settings.php to sites/default/settings.php. Also, in place of the static database credentials, we used the auto-created environment variables.

<pre>
    <code>
        after_build:
            - "mv pagoda/settings.php sites/default/settings.php"
    </code>
</pre>

* Database Component: An empty database was created by adding a db component to the Boxfile.

<pre>
   <code>
        db1:
            name: drupal
   </code>
</pre>

* Database Import: Since the install script creates database tables, it was necessary to import an SQL file. We can do that with a Before Deploy hook, but since that import should only happen on the first deploy and not subsequent deploys, it was placed in the Boxfile.install file.

<pre>
   <code>
        before_deploy:
            - "mysql -h $DB1_HOST --port $DB1_PORT -u $DB1_USER -p$DB1_PASS $DB1_NAME &lt; pagoda/quickstart-db.sql"
   </code>
</pre>