# Intro to IBM Social Business Toolkit

IBM SBT is a java library for interacting with (for example) IBM Connections.

## Where to learn about SBT (or not learn)

 - [ ] SBT is missing it's documentation
 - [ ] SBT is missing it's unittests
 - [ ] IBM Connections REST API
 - [x] Read the code, look at the APIs

## Lessons

It's often easiest to just read the [code](http://mvnrepository.com/artifact/com.ibm.sbt/com.ibm.sbt.core) as the documentation is lacking.

## Endpoint

An endpoint is a way to authenticate your user to Connections. A [BasicEndpoint](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/endpoints/BasicEndpoint.html), uses a username and password to log into Connections.

```java
BasicEndpoint endpoint = new BasicEndpoint();
endpoint.setUrl("connections.item.no");
endpoint.setUsername("username");
endpoint.setPassword("password");
endpoint.setForceTrustSSLCertificate(true);
endpoint.setApiVersion("4.5"); // Use Connections 4.5
```

**Explanation**
  1. Create basic endpoint
  2. Set url to your IBM Connections installation
  3. Set username/password
  4. If your page is https, you can force the client to accept it by setting [setForceTrustSSLCertificate](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/endpoints/AbstractEndpoint.html#setForceTrustSSLCertificate%28boolean%29) to true.
  5. Set the api-version to what your installation in 2. is.

## Blog

This is an example on how to get all the blogs that a user has created. 

```java
public List<Blog> getMyBlogs() throws ClientServicesException  {
  BlogService service = new BlogService(endpoint);
  service.setHomepageHandle("start");
  return service().getMyBlogs();
}
```
**Explanation**
  1. Initialize [BlogService](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/client/connections/blogs/BlogService.html) with an endpoint
  2. The [homepageHandle](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/client/connections/blogs/BlogService.html#setHomepageHandle%28java.lang.String%29) is by default "homepage" and doesn't need to be set. But if it's something else you need to set it.  In our case it's "start". If you don't know what it is; Find it by looking in the Rest-calls in connections. It should be something like; your_connection_url/blogs/homepageHanle, in our case "connections.item.no/blogs/start" 

## ActivityStream 

To get all the activity updates that people do, use ActivityStreamService.

```java
public List<ActivityStreamEntity> readStatus() throws ClientServicesException, ActivityStreamServiceException {
  ActivityStreamService service = new ActivityStreamService(endpoint);
  return service.getStream();
}
```
**Explanation**
  1. Initialize [ActivityStreamService](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/client/connections/activitystreams/ActivityStreamService.html) with an endpoint.
  2. For us [getStream()](http://infolib.lotus.com/resources/social_business_toolkit/javadoc/com/ibm/sbt/services/client/connections/activitystreams/ActivityStreamService.html#getStream%28%29) returns the most relavent activity updates.


