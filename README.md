## Lab 2 - Spring MVC, Dependency injection

### REST

----

REST web servis (često nazivan i REST Web API) je web servis implementiran korišćenjem HTTP protokola i REST (*Representation State Transfer*) principa - [What is REST](http://martinfowler.com/articles/richardsonMaturityModel.html).
REST web servis predstavlja kolekciju **resursa**. Resursima manipulišemo korišćenjem [metoda](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) koje definiše HTTP protokol:

- GET
- POST
- PUT
- DELETE

Korišćenjem ovih metoda API (web servis) se dizajnira tako da je za svaki entitet (resurs)
potreban samo njegov URL za vršenje CRUD operacija preko različitih HTTP metoda:
- ako se traži kolekcija resursa: /activities
- ako se traži konkretan resurs: /activities/{id}, gde id predstavlja identifikator resursa

| Resurs                                      | GET                                                                           | PUT                                                                | POST                                                                                                      | DELETE                 |
|---------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|------------------------|
| Kolekcija http://example.com/resources      | Listaj URI-je i eventualno druge detalja o svim članovima kolekcije.          | Zameni celu kolekciju drugom kolekcijom.                           | Dodaj novog člana kolekcije. URI ovog člana se obično generiše automatski i vraća kao rezultat operacije. | Obriši celu kolekciju. |
| Element http://example.com/resources/item17 | Dobavi traženi element kolekcije u obliku koji je naveden u header-u zahteva. | Zameni navedeni član kolekcije, a ukoliko ne postoji kreiraj novi. | Obično se ne koristi.                                                                                     |Obriši element         |

![REST API](http://46.4.82.177/kursimages/rest_api.svg)

----


### Spring Boot

[Spring Boot](http://projects.spring.io/spring-boot/) se koristi kako bi se pojednostavila konfiguracija Spring projekata. Umesto XML konfiguracionih fajlova, koriste se anotacije u Java klasama. Takođe, na osnovu biblioteka raspoloživih na class path-u, Spring Boot automatski postavlja podrazumevanu konfiguraciju za svaku od njih. Ova konfiguracija se po potrebi može menjati. 

* Obratiti pažnju na izmenjen sadržaj `pom.xml`
* Napraviti klasu WafepaApplication u kojoj će se nalaziti početna Spring Boot konfiguracija. Klasa treba da nasledi `SpringBootServletInitializer`
* Anotirati klasu kao `@SpringBootApplication`
* Dodati ovoj kalsi main metodu
```java
	public static void main(String[] args) throws Exception {
		SpringApplication.run(Wafepa.class, args);
	}
```
* **Pokrenuti klasu kao običnu Java aplikaciju**

* Napraviti datoteku `src/main/resources/application.properties` u kom se nalaze podešavanja za prefiks i sufiks, na osnovu kojih se od imena view-a dolazi do datoteke u kojoj se nalazi njegov šablon.  

```xml
spring.view.prefix: /WEB-INF/jsp/
spring.view.suffix: .jsp
```

### Spring MVC
![Spring MVC](http://46.4.82.177/kursimages/dispatcher.png)

### JSP

JavaServer Pages je template engine namenjen generisanju HTML, XML i drugog tekstualnog sadržaja. Kada se koristi kao view komponenta web aplikacije, svrha JSP-a je da na osnovu šablona i prosleđenog objektnog modela podataka generiše sadržaj koji je moguće prikazati u browser-u. U okviru šablona se za to predviđenim direktivama mogu navesti mesta gde treba umetnuti prikaz određenih podataka. 

![JSP arhitektura](https://upload.wikimedia.org/wikipedia/commons/7/72/JSP_Model_2.svg)

Izvor: Libertyernie2 (Own work) [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0), via Wikimedia Commons

* Napraviti JSP home stranicu (u `src/main/webapp/WEB-INF/jsp` folderu).
* Dodati view-ove: view za pregled svih aktivnosti i brisanje aktivnosti iz te liste.
* Kako bi mogli koristiti JSP i JSTL tagove, na početku JSP stranica importovati ove tagove:

```xml
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

### Servisi i kontroleri

* Napraviti WelcomeController klasu u paketu `jwd.wafepa.web.controller`
* kom će root putanja (`/`) biti mapirana na view sa imenom `home`. Koristiti [@RequestMapping](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html) anotaciju. 

* [Dependency Injection](http://igordejanovic.net/courses/tech/DependencyInjection.html#/) je softverski obrazac koji se koristi kako bi se olakšalo instanciranje objekata. Postoje mnoge implementacije koje se mogu koristiti nezavisno od Spring-a. U okviru Spring Framework-a, DI se koristi kako bi se dobila referenca na neku komponentu

* Da bi Spring Dependency Injecion radio, potrebno je uključiti component-scan mehanizam. U Spring Boot, ovo podešavanje je deo anotacije `@SpringBootApplication` koju smo već primenili. 


* Anotirati `InMemoryActivityService` kao `@Service`

* Napraviti `ActivityController` klasu u paketu `jwts.wafepa.web.controller`
* Injektovati `ActivityService` u `ActivityController` korišćenjem `@Autowired` anotacije
* Definisati mapiranja za HTTP zahteve (`@RequestMapping`)


### Domaći zadatak
Po uzoru na aktivnosti, dodati mogućnost evidencije korisnika:

1. Dodati model klasu User (id, email, password, first name, last name)
2. Dodati servisni sloj za korisnike (interfejs i in-memory implementacija), kao i testove za servisni sloj
3. Dodati zaseban kontroler za korisnike
4. Napraviti zasebne view-ove za evidenciju korisnika