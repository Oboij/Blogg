# IBM Social Business Toolkit SDK

I will refer IBM Social Business Toolkit SDK, to SBT in this blog. 
SBT is a way of interacting with for example IBM Connections as a third-party application. It is a library written in java, with a lot of functions and different uses. 

## Connections + SBT = True?

In past blogs we have talked about how good Connections is, but is Social Business Toolkit equally good? 

There are many pages of documentation that you can read, but that there is a lot of reading it doesn’t necessarily mean that it’s good. I often find it cryptic, that it doesn’t explain how to implement or use SBT and I usually leave more confused than i began with. I found that the best way was just to read all the SBT code and test what works. This takes a lot of time and isn’t the best way of doing it. I shouldn’t complain too much though as I don’t want to write a similar API, that does the same thing and document it.
 I’m going to show you some examples on how to use SBT and which traps not to fall into.

## Endpoint

An endpoint is a way to authenticate your user to Connections. A BasicEndpoint, as shown below, is basicly the same username and password the user logs into Connections with.


```java
public Endpoint login(String username, String password) {
BasicEndpoint endpoint = new BasicEndpoint();
endpoint.setUrl(“www.connections.item.no”);
endpoint.setUsername(username);
endpoint.setPassword(password);
endpoint.setForceTrustSSLCertificate(true);
endpoint.setApiVersion("4.5");
}
```
setForceTrustSSLCertificate(true) is a must, if you want to login to Connections.
apiVersion() needs to be set to the specific version on Connections. If you don’t configure it you will get some really strange issues, trust me.

## Blog

This is an example on how to get all the blogs that a user has created. Hopefully you can just copy/paste it without too much configuration.


```java
public class MyBlogService {

private String blogHomepageHandle;

public MyBlogService(String blogHomepageHandle) {
this.blogHomepageHandle = blogHomepageHandle;
}


public List<Blog> getBlogs()  {
try {
Return service().getMyBlogs();
} catch(ClientServicesException e){
  Logger.error("Can't get Blogs from Connections.", e);
  return Collections.emptyList();
}

}

private BlogService service(){
  BlogService service = new BlogService(login(“usermane”, “password”);
  homepageHandle().ifPresent(service::setHomepageHandle);
  return service;
}
private Optional<String> homepageHandle(){
  return Optional.ofNullable(blogHomepageHandle);
}
private Endpoint login(String username, String password) {
BasicEndpoint endpoint = new BasicEndpoint();
endpoint.setUrl(“www.connections.item.no”);
endpoint.setUsername(username);
endpoint.setPassword(password);
endpoint.setForceTrustSSLCertificate(true);
endpoint.setApiVersion("4.5");
}


}
```
blogHomePageHandle is by default “homePage” and you can set it to null. But for us it was “start”. How to find it isn’t written anywhere and after a few hours of searching we looked in the rest-calls made by Connections and found it! out of sheer luck. If you don’t know where to find a specific page on Connections, look at the rest-calls, don’t bother looking in the documentation.

## ActivityStream 

To get all the status updates that people do, use ActivityStream.

```java

public List<ActivityStreamEntity>
readStatus() throws ClientServicesException, ActivityStreamServiceException {
  ActivityStreamService service = new ActivityStreamService(endpoint);

 return service.getStream();
   

```
ActivityStreamService is in the library provided by SBT and does all the communication. You only need to create an endpoint. 
Everything looks good right? Noting special here… 
But why do I use service.getStream() to get all the status updates? I found out that it was the method that matched what I wanted the best, even though there are methods like getAllUpdates(). As I’m going by trial and error, as I’m sure you do as well, you might find that another method is superior, but who knows?

## Final Thoughts

After I’ve done a few projects with SBT. I still find it very annoying that the documentation is really bad. It makes the development-process slower and sometimes a bit frustrating.On the whole, I think the SBT is okey. If they update the documentation it can become really pleasant to work with. 
I hope that you have learned something  and can learn by my mistakes!
