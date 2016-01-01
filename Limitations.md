# How It Works #

The utplsql plugin converts the configuration tags into an sql script which is used to call the installed utplsql packages. Upon completion of each test the UTR\_OUTCOME table is updated. The table has a description column which is a merged summary of the test executed and any related debug output. The form of the column is:-

```
   <Package Name>.<Method Name>:<Assert Type> <"Test Description" Result>: <Query failure information>.
```
Post execution the contents of UTR\_OUTCOME is retrieved by the plugin and converted in the Surefire xml form.

# Limitations #

The plug-in does its best to decode the merged data and populate the surefire xml output. Unfortunately utplsql is not consistent in the way it populates the description column resulting in some inconsistent output. Below is a sample of some the things utplsql writes into the description field:-

```
  MYBOOKS_PKG.UT_6_DEL: EQQUERYVALUE "ut_del-1" Result: Query "select count(*) from mybooks where book_id=100" returned value "0" that does match "0""
```
```
  PKGTEST.UT_NVSWITHNOCHANGE: Teardown complete
```
```
  .: Unable to run "UTIL".ut_UTIL_CONF.ut_SETUP: ORA-01031: insufficient privileges"
```
```
 PKGTEST.UT_REMIGRATEMODIFIEDMS: EQ "Check USM Services are -1 MS" Expected "43" and got "43"
```
```
 .: EQ "Check Migration Outcome An unexpected error occurred. -1 : ORA-00001: unique constraint (UM_USMSERVICE_FN_I) violated" Expected "0" and got "100"
```

# Solution #

An ideal solution would be for utplsql to be modified to provide messages in a consistent format, perhaps a better solution would be to provide new columns for each fragment of information.

While improving utplsql perhaps a duration column could be added then we could make full use of the surefire xml format.