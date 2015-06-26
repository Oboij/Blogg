# IBM Social Business Toolkit SDK

SBT är ett sätt att integrera med Connections för tredjeparts applikationer. Det är ett  bibliotek skrivit i java, med många funktioner och användningsområden. 

## Connections + SBT = Sant?

Vi har pratat om i tidigare blogginlägg hur bra Connections är, men är Social Business Tool-Kit lika bra?

Det finns många sidor av dokumentation man kan läsa,men bara för att det finns mycket att läsa betyder det inte att den är bra. Ibland kan den vara ganska kryptisk och inte förklara hur man ska implementera eller använda SBT. Utan man får spendera mycket tid med att läsa koden och testa vad som fungerar. Vilket inte allitd är det bästa, eftersom det kan ta ganske lång tid. Nu ska jag inte klaga för mycket. Det är ändå bättre att använda SBT än att skriva en liknande API som gör samma sak.

## Exempel

Hur man kan använda SBT för att hämta alla statusuppdateringar och profilbilder.

```java

public List<StatusMessage> readStatus() throws ClientServicesException, ActivityStreamServiceException {
   ActivityStreamService service = new ActivityStreamService(endpoint.get());

   List<StatusMessage> results = service.getStream().stream()
           .map(Parser::parseStatus).map(s -> statusThumbnail(s))
           .collect(Collectors.toList());

   return results;

private StatusMessage statusThumbnail(StatusMessage status) {
   status.imageURL(imageUtil.getImageURL(connectionsUrl + profleUrl +  userId
               , "profile", cleanId(status)).orElse(null));

   return status;
}


```

ActivityStreamService finns i biblioteket och sköter all kommunikation med Connections. Det man behöver göra är att skapa en endpoint. En endpoint är egentligen i detta fallet användarnamn och lösenord som brukaren använder för att logga in med. Från detta exempel ser allt bra ut med SBT, men ser ni något som är lite underligt? service.getStream() för att hämta alla statusuppdateringar. 
Ja, det är den bästa metoden. Fast det finns metoder som heter getallUpdates() och liknande, är fortfarande getStream() bäst. Är den egentligen bäst? Det är svårt att veta eftersom det inte finns någon bra dokumentation och man får bara testa sig fram. 


## Slutliga kommentarer

Efter att ha gjort flera projekt med SBT, som ni kanske försår efter att ha läst allt, att dokumentationen inte är den bästa. Detta medför att utveckling går sakta och blir stumtals frustrerande. 
Dock är det bra att inte behöva göra arbetet som IBM Social Business Toolkit SDK kommer med. 


