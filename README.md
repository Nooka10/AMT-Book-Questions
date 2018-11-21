# AMT Book Questions
# 1.7 Questions 

```
1. Is Apache Tomcat an application server? The answer to this question depends on the definition of application server. Explain why it is correct to answer positively and negatively to the question.
```





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





```
4. What is the difference between the client tier and the presentation tier. They both seem to be related to the user interface, so why are they both needed?
```

Le tier client contient la partie interface utilisateur. C'est lui qui interagit directement avec le client. Autrement dit, c'est lui que le client utilise directement (interface web, application desktop ou mobile, etc...). Il s'agit donc de la partie *client*.

Les interactions de l'utilisateur dans le tier client engendrent des requêtes vers le tier présentation, généralement en passant par Internet (via HTTP et JSON). Le tier de présentation se trouve donc sur la partie *serveur* de l'application. C'est lui qui est chargé de recevoir les requêtes des clients, les transmettre au tier buisness, attendre la réponse (les données) puis transmettre cette réponse au client.

```
5. Consider the web site https://qoqa.ch. It is reasonable to think that it is built on a multi-tiered architecture. Use the 5-layers diagram and draw the components that might be part of the system.
```





```
6. We have stated that components in the business tier should be decoupled from the user interface. Use the 5-layers diagram and describe a scenario that shows how a business service can be reused across interaction channels.
```





```
7. Most application servers provide 3 ways to deploy a .war file. Describe how these methods work and what are there benefits and drawbacks.
```



# 2.7 Questions

```
1. Let us consider Facebook. What kind of clients does the company use? Do not forget that a company always has external and internal applications.
```

Rich web client et native clients pour les applications mobile. Ils ont aussi probablement un équivalent CLI pour les intégrations aux OS comme dans MacOS... 



```
2. What does the acronym AJAX mean and how does it relate to the client tier?
```

AJAX = Asynchronous JavaScript And XML.

C'est un processus qui se déroule dans le navigateur, côté client et qui permet de faire des requêtes et mettre la page à jour sans avoir à recharger l'entièreté de la page.



```
3. Give an example of a CLI tool that you use and that is the client of a multi-tiered application.
```

Docker...?

# 3.5 questions

```
1. How does one implement HTML form processing in Java EE? Consider the scenario where the user arrives on a page, fills out three fields (first name, last name and e-mail address) and presses a Register button. We want to process the event on the server side and to validate the data entered by the user (the fields cannot be empty and the e-mail address must contain an @ sign). What code do you have to write?
```





```
2. What is the difference between the request, session and application scopes in the servlet API? The method setAttribute is available in the HttpServletRequest, HttpSession and ServletContext classes. What is the difference and when should they be used?
```





```
3. Why do we have to be careful with the HttpSession object?. The Servlet API makes it very easy to store objects in the session. While it is practical to use this feature to build stateful applications on top of the stateless HTTP protocol, it does not come for free. There are at least risks or drawbacks to be aware of. What are they?
```





```
4. How do we deal with relative links in JSP templates?. It is possible to deploy several applications in the same applications server. For this to work, every application is assigned a context root. In order to generate links to CSS files and other assets, it is often necessary to include the context root in the path and it is a bad idea to hard-code the value in the template. What is the solution to this problem?
```

