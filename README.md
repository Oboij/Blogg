# Intro to IBM Social Business Toolkit

IBM SBT is a java library for interacting with (for example) IBM Connections.

## Where to learn about SBT (or not learn)

  * SBT is missing it's documentation
  * SBT is missing it's unittests
  * IBM Connections REST API
  * Read the code, look at the APIs

## Lessons

It's often easiest to just read the [code](http://mvnrepository.com/artifact/com.ibm.sbt/com.ibm.sbt.core) as the documentation is lacking.

## Endpoint

An endpoint is a way to authenticate your user to Connections. A `BasicEndpoint`, uses a username and password to log into Connections.

```java
BasicEndpoint endpoint = new BasicEndpoint();
endpoint.setUrl("connections.item.no");
endpoint.setUsername("username");
endpoint.setPassword("password");
endpoint.setForceTrustSSLCertificate(true);
endpoint.setApiVersion("4.5"); // Use Connections 4.5
```
setForceTrustSSLCertificate(true) is a must, if you want to login to Connections.
apiVersion() needs to be set to the specific version on Connections. If you don’t configure it you will get some really strange issues, trust me.

## Blog

This is an example on how to get all the blogs that a user has created. Hopefully you can just copy/paste it without too much configuration.


```java

public List<Blog> getBlogs() throws ClientServicesException  {
  BlogService service = new BlogService(endpoint);
  service.setHomepageHandle("start");
  return service().getMyBlogs();
}
```
blogHomePageHandle is by default “homePage” and you can set it to null. But for us it was “start”. How to find it isn’t written anywhere and after a few hours of searching we looked in the rest-calls made by Connections and found it! out of sheer luck. If you don’t know where to find a specific page on Connections, look at the rest-calls, don’t bother looking in the documentation.

## ActivityStream 

To get all the status updates that people do, use ActivityStream.

```java
public List<ActivityStreamEntity> readStatus() throws ClientServicesException, ActivityStreamServiceException {
  ActivityStreamService service = new ActivityStreamService(endpoint);
  return service.getStream();
}
```
ActivityStreamService is in the library provided by SBT and does all the communication. You only need to create an endpoint. 
Everything looks good right? Noting special here… 
But why do I use service.getStream() to get all the status updates? I found out that it was the method that matched what I wanted the best, even though there are methods like getAllUpdates(). As I’m going by trial and error, as I’m sure you do as well, you might find that another method is superior, but who knows?

## Final Thoughts

After I’ve done a few projects with SBT. I still find it very annoying that the documentation is really bad. It makes the development-process slower and sometimes a bit frustrating.On the whole, I think the SBT is okey. If they update the documentation it can become really pleasant to work with. 
I hope that you have learned something  and can learn by my mistakes!
