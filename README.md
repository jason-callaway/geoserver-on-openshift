GeoServer on OpenShift
======================

View this live at [http://geopaas.io](http://geopaas.io).

To fully appreciate what is happening here please read the [blog post](https://www.openshift.com/blogs/build-your-own-google-maps-and-more-with-geoserver-on-openshift) by Steven on which this demo is based.

Create the gear.

```
rhc app-create -s geoserver jbossews-2.0
rhc cartridge add postgresql-9.2 --app geoserver
```

Add the PostGIS extensions to PostgreSQL.

```
rhc ssh geoserver
psql
create extension postgis;
\q
exit
```

Pull down the demo environment

```
cd geoserver
git remote add github -m master git@github.com:jason-callaway/geoserver-on-openshift.git

git pull -s recursive -X theirs github master
```

We don't need to build anything, so remove pom.xml.

```
git rm pom.xml
```

Now commit and push

```
git commit -am 'initial commit'

git push origin
```

Set CATALINA_OPTS.
```
rhc set-env --env CATALINA_OPTS=/var/lib/<your uuid>/app-root/data/geoserver_data --app geoserver
```

The GeoServer can be accessed at http://geoserver-yournamespace.rhcloud.com/web.
