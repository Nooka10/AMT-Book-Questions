# AMT Book Questions
# 1.7 Questions 

```
1. Is Apache Tomcat an application server? The answer to this question depends on the definition of application server. Explain why it is correct to answer positively and negatively to the question.
```

Tomcat n'est pas considéré comme un server d'application à part entière car il n'implémente pas entièrement la spécification de JavaEE, mais utiliser TomEE, comble l'écart entre Tomcat et la spécification JavaEE.

```
2. What is the role of a .war file? Explain how it fits in the Java EE development model and bridges the gap between development and operations.
```

Autrefois, les équipes étaient séparées entre les développeurs et les ingénieurs IT. Les développeurs écrivaient le code des composants et des fonctionnalités et créaient les applications tandis que les ingénieurs IT s'occupaient de les rendre disponible, les déployaient et s'assuraient u'elles soient constemment disponibles et fonctionnelles.

Lors de mise à jour de l'application par les développeurs, il fallait un modèle de fichier facile à transmettre au ingénieurs IT pour qu'ils puissent facilement le déployer.

3 packages ont été créés pour cela:

- Les fichiers .war étaient proposés pour les applications web. L'idée était qu'ils contient le s composants du tier de présentation, soit les composants processant les requêtes HTTP, récupérant les données et générant les vuew HTTP à envoyer en réponse.
- Les fichiers .jar étaient proposés pour contenir les composants de logique métier (buisness tier). L'idée était de séparer les composants buisness du tier de présentation afin de pouvoir facilement les réutiliser. 
- Les fichiers .ear étaient proposés pour contenir l'ensemble de l'application (.war et .jar en un seul fichier).

Au début de Java EE, les choses se sont beaucoup faites ainsi, mais avec l'évolution de Java EE et les habitudes prises par les différentes équipes cela a un peu changé.

Aujourd'hui, on n'utilise presque plus que des .war.

```
3. Is the J2EE development model an anti-pattern? On paper, the clear separation between development and operations teams makes sense. However, the current trends in the industry are very different. Explain how modern practices give a different perspective on the question.
```

De nos jour, on tend de plus en plus à casser la barrière séparant les développeurs des opérateurs IT, et on tente de travailler avec des équipes plus petites. C'est le principe de l'approche DevOps.

```
4. What is the difference between the client tier and the presentation tier. They both seem to be related to the user interface, so why are they both needed?
```

Le tier client contient la partie interface utilisateur. C'est lui qui interagit directement avec le client. Autrement dit, c'est lui que le client utilise directement (interface web, application desktop ou mobile, etc...). Il s'agit donc de la partie *client*.

Les interactions de l'utilisateur dans le tier client engendrent des requêtes vers le tier présentation, généralement en passant par Internet (via HTTP et JSON). Le tier de présentation se trouve donc sur la partie *serveur* de l'application. C'est lui qui est chargé de recevoir les requêtes des clients, les transmettre au tier buisness, attendre la réponse (les données) puis transmettre cette réponse au client.

```
5. Consider the web site https://qoqa.ch. It is reasonable to think that it is built on a multi-tiered architecture. Use the 5-layers diagram and draw the components that might be part of the system.
```

Le site web https://qoqa.ch est probablement une application multi-tier.

| Client                           | Présentation                      | Buisness                                                     | Integration                                                  | Ressources                                                   |
| -------------------------------- | --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| La page web sur notre navigateur | Les servlets gérant les endpoint. | Service gérant le shopping cart.<br />Service gérant les transactions bancaires | Liaison avec la base de donnée.<br />Liste de livraison , etc | Base de donnée MySQL ou PostGres. (probablement relationnel) |

```
6. We have stated that components in the business tier should be decoupled from the user interface. Use the 5-layers diagram and describe a scenario that shows how a business service can be reused across interaction channels.
```

| Client | Présentation | Buisness | Integration | Ressources |
| ------ | ------------ | -------- | ----------- | ---------- |
|        |              |          |             |            |



```
7. Most application servers provide 3 ways to deploy a .war file. Describe how these methods work and what are there benefits and drawbacks.
```

- Par l'interface web (Serveur d'application)
  - Facile à faire
  - Très peu utilisé en pratique
- Par ligne de commande
  - Utilisable dans des scripts
  - (IntelliJ utilise ça pour déployer depuis son interface graphique)
  - asadmin pour payara (glassfish)
- Au démarrage du serveur.
  - On peut définir un dossier contenant les applications à déployer automatiquement au démarrage du serveur.
  - Utile pour la création de container docker.

# 2.7 Questions

```
1. Let us consider Facebook. What kind of clients does the company use? Do not forget that a company always has external and internal applications.
```

*Rich web client* et *native clients* pour les applications mobile. Ils ont aussi probablement un équivalent CLI pour les intégrations aux OS comme dans MacOS... 

```
2. What does the acronym AJAX mean and how does it relate to the client tier?
```

AJAX = Asynchronous JavaScript And XML.

C'est un processus qui se déroule dans le navigateur, côté client et qui permet de faire des requêtes et mettre la page à jour sans avoir à recharger l'entièreté de la page.

```
3. Give an example of a CLI tool that you use and that is the client of a multi-tiered application.
```

Docker, heroku CLI, kubctl

# 3.5 questions

```
1. How does one implement HTML form processing in Java EE? Consider the scenario where the user arrives on a page, fills out three fields (first name, last name and e-mail address) and presses a Register button. We want to process the event on the server side and to validate the data entered by the user (the fields cannot be empty and the e-mail address must contain an @ sign). What code do you have to write?
```

- Le formulaire ce trouve sous forme de jsp sur le serveur.

- Lorsque le client fait une requête sur un endpoint demandant ce formulaire, on va construire une page HTML (avec son css et js associés) et ajouter les valeurs par défaut (s'il y en a), en parcourant les différents scope (request, session, application).

- Lorsque l'utilisateur aura rempli le formulaire, il appuiera sur le bouton *send* qui fera une requête POST sur le servlet. Ce dernier contrôlera la validité des entrées utilisateur, retournera des erreurs si les données sont invalides, ou redirigera la requête vers un autre servlet qui donnera une nouvelle page.

  ```Java
  @WebServlet(name = "RegisterServlet", urlPatterns = {"/pages/register"})
  public class RegisterServlet extends HttpServlet {
  
      @EJB
      UsersDAOLocal usersDAOLocal;
  
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          String email = request.getParameter("email");
          String lastName = request.getParameter("lastName");
          String firstName = request.getParameter("firstName");
  
          MessageServeur message = new MessageServeur(); // Class containing error
  
          if (!(lastName.length() > 0)) {
              message.addErrorMessage("Name can't be empty.");
          }
  
          if (!(firstName.length() > 0)) {
              message.addErrorMessage("FirstName can't be empty.");
          }
          
          if(!(email.length() > 0)){
              message.addErrorMessage("Email can't be empty");
          }
  
          // Redirige sur home si pas d'erreur
          if (!message.haveError()) {
              User user = new User(lastName, firstName, email);
              usersDAOLocal.addNewuser(user);
              request.getRequestDispatcher("/WEB-INF/pages/home.jsp").forward(request, response); // redirige vers la page d'acceuil
          } else { // Redirige sur la page du formulaire en montrant les erreurs
                  request.setAttribute("error", message.getErrorMessage());
                  request.setAttribute("email", email);
                  request.setAttribute("lastName", lastName);
                  request.setAttribute("firstName", firstName);
  
                  request.getRequestDispatcher("/WEB-INF/pages/register.jsp").forward(request, response);
              } 
          }
      }
  
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          request.setAttribute("email", "");
          request.setAttribute("pageTitle", "Register");
          request.getRequestDispatcher("/WEB-INF/pages/register.jsp").forward(request, response);
      }
  }
  ```

```
2. What is the difference between the request, session and application scopes in the servlet API? The method setAttribute is available in the HttpServletRequest, HttpSession and ServletContext classes. What is the difference and when should they be used?
```

- Request
  - Relatif à une requête.
  - Contient les paramètres de la requête. (Ex: id de l'applications à afficher, URL, ...)
  - Est transféré sur le réseau donc attention au données personnelles ou confidentielles
  - La request passe dans différents services (sevlet, filter, jsp, etc). C'est un bon moyen pour faire passer des informations entre ces différents services.
- Session
  - Relatif à la session d'un utilisateur.
  - Contient les infos de l'utilisateur.
  - Reste du coté serveur.
  - On y stocke par exemple le status, l'id, l'email etc...
  - Permet d'éviter des requêtes à la base de donnée mais doit rester petit!
    - Si on stocke trop d'informations dans la session, on risque d'avoir des problèmes si notre site a beaucoup d'utilisateurs connectés en même temps --> Disque de surcharge de la RAM --> plantage du serveur... 
  - Le client reçoit un cookie qui ne contient pas les données de la session mais uniquement un identifiant.
- Application
  - Partagé dans toute l'application

```
3. Why do we have to be careful with the HttpSession object? The Servlet API makes it very easy to store objects in the session. While it is practical to use this feature to build stateful applications on top of the stateless HTTP protocol, it does not come for free. There are at least risks or drawbacks to be aware of. What are they?
```

- On doit faire attention à ne pas stocker des informations trop lourdes dans la session HTTP car cela peut poser problème lorsque le site monte en charge...
- Si beaucoup d'utilisateurs en simultanés, risque de surcharge de le RAM --> plantage du serveur...

```
4. How do we deal with relative links in JSP templates?. It is possible to deploy several applications in the same applications server. For this to work, every application is assigned a context root. In order to generate links to CSS files and other assets, it is often necessary to include the context root in the path and it is a bad idea to hard-code the value in the template. What is the solution to this problem?
```

Ils faut définir les chemin dans le web.xml ? // A confirmer

# 4.0 Question bonus

```
Why manage conversational state in a SFSB rather than in an HTTP session? 
```

Un argument est le besoin du support de clients riches qui ne passent pas par le tier de présentation mais appellent directement l'EJB (tier Buisness).

Un autre argument est la séparation entre l'interface utilisateur durant des worflow utilisant différents clients.

