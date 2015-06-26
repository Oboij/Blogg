# IBM Social Business Toolkit SDK

SBT �r ett s�tt att integrera med Connections f�r tredjeparts applikationer. Det �r ett  bibliotek skrivit i java, med m�nga funktioner och anv�ndningsomr�den. 

## Connections + SBT = Sant?

Vi har pratat om i tidigare blogginl�gg hur bra Connections �r, men �r Social Business Tool-Kit lika bra?

Det finns m�nga sidor av dokumentation man kan l�sa,men bara f�r att det finns mycket att l�sa betyder det inte att den �r bra. Ibland kan den vara ganska kryptisk och inte f�rklara hur man ska implementera eller anv�nda SBT. Utan man f�r spendera mycket tid med att l�sa koden och testa vad som fungerar. Vilket inte allitd �r det b�sta, eftersom det kan ta ganske l�ng tid. Nu ska jag inte klaga f�r mycket. Det �r �nd� b�ttre att anv�nda SBT �n att skriva en liknande API som g�r samma sak.

## Exempel

Hur man kan anv�nda SBT f�r att h�mta alla statusuppdateringar och profilbilder.

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

ActivityStreamService finns i biblioteket och sk�ter all kommunikation med Connections. Det man beh�ver g�ra �r att skapa en endpoint. En endpoint �r egentligen i detta fallet anv�ndarnamn och l�senord som brukaren anv�nder f�r att logga in med. Fr�n detta exempel ser allt bra ut med SBT, men ser ni n�got som �r lite underligt? service.getStream() f�r att h�mta alla statusuppdateringar. 
Ja, det �r den b�sta metoden. Fast det finns metoder som heter getallUpdates() och liknande, �r fortfarande getStream() b�st. �r den egentligen b�st? Det �r sv�rt att veta eftersom det inte finns n�gon bra dokumentation och man f�r bara testa sig fram. 


## Slutliga kommentarer

Efter att ha gjort flera projekt med SBT, som ni kanske f�rs�r efter att ha l�st allt, att dokumentationen inte �r den b�sta. Detta medf�r att utveckling g�r sakta och blir stumtals frustrerande. 
Dock �r det bra att inte beh�va g�ra arbetet som IBM Social Business Toolkit SDK kommer med. 


