# IBM Social Business Toolkit SDK

SDK är ett sätt att integrera Connections med tredjeparts applikationer. Det är ett  bibliotek skrivit i java, med många funktioner och användningsområden. 

## Connections + SDK = Sant?

Vi har pratat i tidigare blogginlägg hur bra och ledande i sitt område Connections är, men är Social Business Tool-Kit lika bra?

Det finns många sidor dokumentation som man kan läsa, men bara för att det finns mycket att läsa betyder det inte att den är bra. Ibland kan den vara ganska kryptisk och inte förklara hur man ska implementera eller använda SDK. Utan man får spendera mycket tid med att läsa koden och testa vad som fungerar. Vilket inte allitd är det bästa, eftersom det kan ta ganska lång tid. Nu ska jag inte klaga för mycket. Det är ändå bättre att använda SDK än att skriva en liknande API som gör samma sak.

## Exempel

Hur man kan använda SDK för att hämta alla statusuppdateringar och profilbilder.

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

ActivityStreamService finns i biblioteket och sköter all kommunikation med Connections. Det man behöver göra är att skapa en endpoint. En endpoint är egentligen, i detta fallet, användarnamn och lösenord som brukaren använder för att logga in i Connections med. Från detta exempel ser allt bra ut med SDK, men ser ni något som är lite underligt? service.getStream() för att hämta alla statusuppdateringar. 
Ja, det är den bästa metoden. Fast det finns metoder som heter getallUpdates() och liknande, är fortfarande getStream() bäst. Är den egentligen bäst? Det är svårt att veta eftersom det inte finns någon bra dokumentation och man får bara testa sig fram. 


## Slutliga kommentarer

Efter att ha gjort flera projekt med SDK, som ni kanske försår efter att ha läst allt, att dokumentationen inte är den bästa. Detta medför att utveckling går sakta och blir stumtals frustrerande. 
Dock är det bra att inte behöva göra arbetet som IBM Social Business Toolkit SDK kommer med. 
I det stora hela tycker jag att SDK fungerar okej och om de uppdaterar dokumentationen kommer utveckling med SDK gå snabbt och smidigt. 
Om ni vill använda SDK själva, eller bara är intresserade i att veta mer så kan ni se deras Github; https://github.com/OpenNTF/SocialSDK

