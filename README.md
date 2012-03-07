#FreeGeoIP.net Bundle for Laravel Framework.

###DISCLAIMER:

The developer of this Laravel bundle is not affiliated with FreeGeoIP.net.

###WHAT IS IT?

A bundle for the Laravel Framework (http://www.laravel.com).

This bundle provides a static class which retrieves geo location
based on the user's IP address through the freegeoip.net API.
(http://www.freegeoip.net).

Thanks to FreeGeoIP.net you can, with some degree of accuracy retrieve
the following information about your users within the Laravel framework.

    - Country
    - 2-digit Country Code
    - City
    - Region (Province, State)
    - 2-digit Region Code
    - Latitude
    - Longitude

NOTE:
There is a limit of 1000 requests per hour per client on the FreeGeoIP API.

#INSTALLATION:

###PREREQUISITES:

Sessions configured and active, the FreeGeoIP class will use a
session variable called 'geo' to store and timestamp the information
it retrieves.

###ARTISAN:

    php artisan bundle:install freegeoip

###|| DOWNLOAD:

    (download link to go here)

###ADD TO BUNDLES:

Open your application/bundles.php file, add 'freegeoip' the array:
    
      return array(
          'freegeoip' => array(),
      );

Alternatively, auto-load the bundle.

      return array(
          'freegeoip' => array(
            'auto' => true,
          ),
      );

#USAGE:

Anywhere it's required, start the bundle.

    Bundle::start('freegeoip');

That's it. Now just reference the geo data. All of the following are
available.

    echo FreeGeoIP::city();
    echo FreeGeoIP::country_name();
    echo FreeGeoIP::country_code();
    echo FreeGeoIP::region_name();
    echo FreeGeoIP::region_code();
    echo FreeGeoIP::longitude();
    echo FreeGeoIP::latitude();

If you would like to force a refresh of the geo data you can, like this.

    FreeGeoIP::refresh(true);

If you would like to forget the geo data, you can, like this.

    FreeGeoIP::forget();

If you would like to simply access the session data directly, you can.

    Session::get('geo')->city;
    Session::get('geo')->country_name;
    Session::get('geo')->country_code;
    Session::get('geo')->region_name;
    Session::get('geo')->region_code;
    Session::get('geo')->longitude;
    Session::get('geo')->latitude;

##CONFIGURATION OPTIONS:

Being there are only 2 variables you may want to tweak I opted to
include them in the class rather than a separate config file. </lazy>

    1. Open bundles/freegeoip/FreeGeoIP.php
    2. Modify the $curltimeout and $refreshInterval variables as desired.
  
##HOW IT WORKS:

Q: But if I have a lot of users, won't making a CURL call everytime
   the user loads a page affect the performance of my site?

A: Yes, so we timestamp the geo data and compare it against an interval.

    -> Bundle starts
    -> Bundle looks for 'geo' session variable
    -> Bundle compares 'geo' timestamp to current time
    -> If refreshInterval has elapsed, a refresh is triggered
    -> Optionally you may call FreeGeoIP::refresh(true) to force it
    -> Geo data is exposed through session variable as well as
       magic methods on the FreeGeoIP class.

The bundle provides a static class called "FreeGeoIP". Curl is used to
retrieve geo information from FreeGeoIP.net and is then timestamped and
stored in a session variable called 'geo'. Each time the bundle starts
it checks the age of the current session 'geo' data and compares it to
the configured $refreshInterval. Default is set to 30 minutes. If the
interval is exceeded the class will automatically refresh the data, else
it simply leaves the existing session 'geo' data as is.

Because magic static methods are fun, you don't have to use the session
variable directly to access the location data... but you can.