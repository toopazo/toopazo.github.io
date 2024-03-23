---
title: ¿Porque usar Github Pages y Jekyll para una página web tipo Blog?
layout: post
---

[WordPress](https://wordpress.com/) es un gestor de paginas web escrito en PHP e integrado a bases de datos locales para almacenar el contenido en si. Su código fuente es open-source y su uso es gratuito [[1](https://en.wikipedia.org/wiki/WordPress)]. Contiene cientos de miles de ejemplos y extensiones de todo tipo para ser usado en miles de casos diferentes. Existe un ecosistema completo girando al rededor de su uso, desde los servidores que lo alojan (Hostinger, Bluehost, Ionos y miles de miles más) hasta creadores de plugins que lo complementan.  Se estima que cerca del 40% de todos los sitios web disponibles en internet operan usando WordPress [[2](https://www.hostinger.com/tutorials/why-use-wordpress?)]. Por todo lo anterior vale la pena invertir la pregunta y cuestionarse: ***¿porque no usar WordPress?***. 

Pues sucede que existe un pequeñito pero importante nicho de usuarios para los que la respuesta es diferente. Esto se debe, a mi juicio, debido a dos factores: costo y publico objetivo. 

Hoy en dia (Marzo 2024) existen planes para alojar WordPress desde los 3 USD al mes [[3](https://www.bluehost.com/wordpress/wordpress-hosting?#reviewSignal)]. A este costo se le debe sumar el dominio DNS asociado a la pagina web, el cual puede rondar otro 5 USD al año [[4](https://www.domain.com/domains?)]. Este costo es bajísimo y muy fácil de absorber, sin embargo su costo no es cero, y eso importa. Si queremos gastar 0, zero, cero, null, entonces, debemos seguir buscando.

El segundo motivo tiene que ver con el publico objetivo. Si las personas que van a crear contenido y administrar la página web son las mismas (1), y si su número no superan las 3 a 5 personas (2), y si además son usuarios de Github, Gitlab, o cualquier otro versionador de código (3), entonces ¡tenemos un candidato para usted!: [Github Pages](https://pages.github.com/) + [Jekyll](https://jekyllrb.com). 


### ¿Como funciona este dúo?

Github Pages es un servicio gratuito ofrecido por [Github](https://github.com/) que lleva más de  15 años alojando miles de sitios web simples, como documentación de proyectos de software, blogs e iniciativas de investigación académica y privada. A pesar de que su uso esté limitado a contenido estático y relativamente simple, su uso dentro de proyectos de desarrollo e investigación es muy extendido. Y para evitar tener que escribir nuestra página web en HTML de manera directa, Github Pages escogió a Jekyll como la plataforma recomendada para facilitar las cosas. Esta plataforma es un generador de páginas web enfocada a Blogs que promete simplificar su creación, diseño y uso. Jekyll es una herramienta parecida a WordPress aunque mucho más limitada en sus aplicaciones. Sin embargo es más simple de entender y usar para los usuarios que normalmente usan Github. Para más detalles se recomienda visitar [about-github-pages-and-jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll).

Afortunadamente en Jekyll existen muchísimas plantillas disponibles, tanto pagados como gratuitas, en las que podemos basar nuestro sitio web sin tener que empezar a escribir código HTML o CSS. Estas plantillas permiten elegir entre diferentes estilos visuales y de organización de la información contenida. Por último Jekyll permite incorporar una seria de extensiones (plugins) a nuestra página web de manera de potenciarla con funciones como: manejo de idiomas, interacción con buscadores, manejo de bibliografía y citas, paginación, imágenes y videos, etc. Todo muy parecido a WordPress, aunque a menor escala. 

### ¿Como funciona Jekyll?
El punto de partida para aprender el uso de cualquier herramienta de software debiese siempre ser la página de documentación del grupo o empresa que la desarrolla. En este caso la norma afortunadamente se cumple de buena forma, por lo que no me queda más que recomendar una lectura de la página [https://jekyllrb.com/docs/](https://jekyllrb.com/docs/).

Jekyll funciona integrando Ruby con un par de lenguajes de plantilla ([templating lenguage](https://collectiveidea.com/blog/archives/2018/03/06/what-s-in-a-templating-language-part-1)): 
- ***Markdown*** para escribir el contenido del sitio web en sí. 
- ***Liquid*** para escribir el código que define donde y como desplegar cada componente del cada pagina del sitio web (header, footer, sliders, sidebars, etc).
- ***Ruby*** para ejecutar los dos lenguajes anteriores y llevarlos a HTML y CSS para ser desplegados en un servidor. 

### Notas al cierre
Dentro del nicho definido de usuarios de Github que no quieren pagar un peso y a los que no les molesta la aridez de la programación, el uso de Github Pages + Jekyll es la mejor alternativa hasta la fecha (Marzo 2024) para tener un Blog simple de manejar y visualmente atractivo para mostrarle al mundo. Esta página web es testimonio de lo antes dicho (eso espero al menos). 

<center>
¡Saludos!
</center>

<!-- ### Notas de uso

```bash
bundle exec jekyll serve --livereload
``` -->