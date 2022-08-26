# CMSDockerDev

This project's goal is to serve common examples of containerized deployments for quick-start CMS (Content Management System) websites.


<!-- Covered examples include:

Not entirely limited to just CMS type sites; include other non-developer set-ups for convenient access use cases

    PHP:
        Wordpress
        Drupal

        # dev framework
        Laravel

    Python:
        Wagtail

    CSharp:
        Orchard

    Javascript:
        WikiJs



Notes and Documentation:

    Wordpress:
        https://hub.docker.com/_/wordpress/

    PhpMyAdmin:
        https://hub.docker.com/_/phpmyadmin?tab=description


    Drupal:
        https://www.drupal.org/docs/security-in-drupal/securing-file-permissions-and-ownership
        https://www.drupal.org/server-permissions
        https://hub.docker.com/_/drupal


    Orchard:
        https://docs.orchardcore.net/en/latest/
        
    Wagtail:
        https://github.com/wagtail/bakerydemo

    
Initial issue tracking:

drupal mounting local storage results in permissions errors with drupal storage on /var/www/html -> /opt/drupal/web because directory requires ownership by www-data; issue isn't related solely to permissions, the contents may be overwritten by the hosts mounted directory

Solution: using docker volumes to address necessary container storage
 -->