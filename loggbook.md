# Rapport för Openshift - ArgoCD projekt

## 2026-01-15
    Idag har jag sutit och försökt att få igång alla de fya Containers som frontend, backend, Postgresql och Redis. Men det tog mig tid och jag klarade att bygga en container för Frontend och testade att bygga Backend i terminalen bara att för att se om jag får "healthy" för min postgresql och redis. 

    Frontend gick bra men när det kom till backend, postgresql och redis var det svårt  få de kommunicera. 
    Problemet var att jag hade inte skapat nätverk och det var självklarhet att det inte gick men sen när jag väl lärde mig, tog jag bort backend containern och och byggde om det men "--network" flaga

## 2026-01-16
    Dagens mål var att jag och Jonas ska kunna skapa container filer för Backend, Postgresql och Redis och bygga de i vårat repo. Det blev mycket enkklare när jag hade redan testat det i terminalen dagen innan och visste hur byggar man till exampel en postgresql container och vilka variablar behöver man där. 

    Men det som jag stötte på och tog längre tid var om hur ska jag bygga nätverket för de i koden. I terminalen så skrev jag podman create --network guestbook-net och det gick fort. Efter en stund undersökningar så fick jag reda på att när jag är klar med skriva de tre Conatinerfiles då kan jag bara lägga till nätverk i det kommandot som jag startar alla mina Conatiner med. Detta gör att jag kan ha alla fyra Conatiner under en och samma nätverk.
    Exampel på Comandot "podman run -d --name Contanier-name --network nework-name -p port Container-Name"

## 2026-01-17
    Idag har jag och Jonas sutit och samtalt alla och försökte få igång application och kunna skriva i frontend och skicka den vidare. samt att den ska sparas så länge continerna inte är borttagen. 

    Problemet idag var att vi kunde komma till frontend och backend i terminalen fick vi healthy med hjälp av curl men när vi skrev i hemsidan så kunde den inte skicka eller spara. 

    Vi har undersökt i flera timmar och kollat i google och AI och till slut fick vi reda på att i main.go bevhöver vi specificera databasen med postgresql och även för redis behöver vi specificera redis, port och lösenord. Sedan sätte vi resolver proxy i nginx.conf till localhost och testade igen.

    Nu har vi igång alla container och allt sparas så länge man inte ta bort Containers från podman. 


## 2026-01-19
    Jag och Jonas har sutit idag och dokumenterad samt fyllde i logbooken. Vi har diskuterat om hur hade vi kunnat lösa det på ett annat sätt, om det finns ett annat sätt och hur hade det påverkat produktionen. Vi har några ideér men vi vill gärna diskutera det imorgon när vi har lektion och träffas på plats med Jonas Björk (Läraren). 

## 2026-01-20
    Tisdag idag och under lektionen fick vi att jobba med skapa workflows för frontend och backend. Jag och Jonas tänkte att bygga båda podman och docker baserad workflows för att se om det kommer funka eller inte. 
    Vi började med frontend först och Jonas valde docker och jag började med att bygga frontend workflowet i podman för att se hur det reagerar och om det ens gå. 

    Jag tänkte att eftersom vi har jobbat med podman sen början så kan det vara bra att fortsätta med podman även i workflows om det går. 
     - Jag behövde ha ett steps för att isntallera och Podman allra först efter att den har     gjort checkout. 
     - logga in på Github container registry med Podman
     - Bygga imagen
     - Pusha imagen

    När jag har kommitat ändringar och pushat märkte vi att workflowet gick inte genom och under loggar stod tydlig att mitt ´username´ på Github börja med storbokstav (Narah111).
    Jag har aldrig haft detta problem men jag började googla och se om jag hittar en lösning och efter stund tog jag och Jonas en dialog med Läraren. Till slut har vi hårdkodade usernmet i image prefix med lite bokstav och då gick min workflows genom. 

    Vi har lärt oss något nytt i den delen och även provat att frontenden går och köra på podman.


    Vi började lite med Backend men då var dagen slut och vi bestämde att fortsatta imorgon och vi ska träffas på skolan själva.

## 2026-01-21
    Dagen mål är att jag och Jonas ska sätta upp även backend workflowet så att vi är ikapp tills torsdagen lektion. 

    Vi började med samma plan att Jonas ska bygga backend med att docker och jag ska bygga den med podman. Vi tänkte prova att skriva samma workflows som frontend fast ändra vissa värden som build bakcend image osv bara för att se om det funkar. 

    Det funkade tyvärr inte vilket är inte så konstigt och vi förväntade oss det.
    Efter en stund google och AI fråga stund fick vi redan på att vi behöver Qemu setup sedan definera platforms under build och push steps då kunde vi bygga imagen i både Linux och Mac miljö(amd64 & arm64) och testade workflowes och det gick. Fast det 8-9min. 

    Efter en stund diskution, kom Jonas med förslaget att vi får rita på tavlan hela flödet från början till slut för att se hur vi tänker och hur kan vi undvika den långa vänte tiden. 

![IMG_2979](https://github.com/user-attachments/assets/80745282-4de9-42a2-9cbe-03b76e182d31)


    Som bilden visar så har vi tänkt att vi ska ha en future branch (development) och bygga backend workflows att när det är push till dev ska den bygga bara arm64 och när det är PR till main ska den bygga amd64 för så att vi kan använda den i Openshift. Vi tror att med hjälp av den här fördelningen kan vi spara både tid och gör det mer specificerad. 

    Juste, jag började bygga backenden med Docker då fick jag inga bra rekomendationer i framtiden varken från läraren eller i google. 

    Jag och Jonas har skrivit om backenden i två olika sätt men samma funktion och efter ett par timmars slit lyckades vi ha en fungerande backend som vi önkade enligt bilden.

## 2026-01-22 - 2026-01-23 2026-01-26
    Den 22 januri hade vi lektion och denna lektionen gick läraren genom Openshift generellt och de viktiga funktioner som vi kommer behöver använda i vårat prjekt. 
    Den 23de(fredag) och 26te(måndag) fick vi jobba med labben i RedHat som vi kan öva på den en hel del. 

## 2026-01-27
    Lektion idag, men jag tyvärr har missat det pga av sjukdom. 

## 2026-01-28
    Idag mår jag bättre och gick till skolan för att jobba själv och försöka komma ikapp. Jag sätte mig med JonasF och försökte skriva in de yml filer som Jonas läraren har gått genom för att kunna bygga de yml manifests.
    
    Eftersom Jonas läraren har gått genom det mesta dagen innan så alla filer var färdiga och vi har inte stött på något problem men det var ändå lärorik att klicka runt i openshift. Riktigt bra att tillfälle att vi fick möjlighet att lära känna miljön innan vi börjar bygga de manifest ihop med våran app. 

## 2026-01-29
    Jonas (läraren) har gått genom om hur kan man bygga de manifest i ett eget repo och bygga den i ArgoCD. Idag fick vi möjlighet att se hur app repo och och infra repo kommunicerar och vad är det som viktigt att tänka på när man bygga de manifest.

    Jag har skapat en .gitignore fil där jag har specificerad att alla secrets och configmap filer ska inte pushas för hålla repot så säkert som möjligt.

    Jag har lyckats att bygga infra repot och bygga upp yml manifest. Men sen fick jag ett problem som tog mig resten av dagen att felsöka och ännu längre.
    Problemet var att min workflows byggde images och ArgoCD hitta en ny images tag men det var tom eller lättare sagt det image tagen var tom.

## 2026-01-30 
    JonasF har blivit sjuk och jag fick själv sitta och felsöka. Jag började med att gå genom mina workflows för att se hur det byggar images och det får de nya tags. 
    
    Jag kunde inte hitta se och jag började googla och fråga cloude AI men jag AI var gav mig information där jag kände att det här kan inte stämma. Jag vågade inte göra var AI säger så jag vände mig till JonasF och kollade om han orkar felsöka med mig. 

    Efter en bra stund kunde jag lösa probleme med hjälp av JonasF.
    Problemet var att jag först skapat en image tag med och nästa step när det byggde images och push där had hade jag inte specificerad att när imagen byggs ska den innehålla den nya tagen.
    Det är för att i sista steps "update Kubernetes manifest" säger jag till det repot att hämta imagen som har den specifika tag och sha koden `steps.image-tag.outputs.tag`.

    Nu fick jag rätt image både för frontend och backend. Jag har testad appen i webläsaren och jag allt funkar som det ska. 

## 2026-01-31
    Idag fick jag hälpa Alma meg hennes projekt i tre timmar och klickade runt och föröskte lära mig lite.

## 2026-02-02
    Jag skulle uppdatera loggboken idag och bara pusha den men efter att jag var klar och pushade till main så märkte jag att argoCD hittar inte image med amd64 version(samma problem som i torsdags). Jag visste direkt att det är min backend workflows som byggar imagen i fel version. Jag konaktade JonasF för att kolla om hur `workflow-dispatch` fungerar. Jag hade redan i min workflow dispatch men den funkade tydligen inte för att den byggde imagen med arm64.
    Vi pratade genom discord. 
    - Han rekomenderade att jag ska ha backend path on både push och pull request vilket jag inte hade.
    - Vi fixade även Qemu setup så att den ska köra om workflows är på dispatch annars skipa(detta är bara för spara tid och inte automatisk)
    - Vi har lagt även en if funktion för update kubernetes manifest så att så länge det inte är dispatch ska den körs automatisk. 
## 2026-02-08 

    Jag har tänkt lite på att hur skulle min workflows kan bli bättre. Jag skulle kunna ha mer tests för att koden ska vara i säker sidan innan den pushas samt linting skulle vara bra för att se att all syntax och de regler som jag önksar passar med koden innan den pushas till produktion.

