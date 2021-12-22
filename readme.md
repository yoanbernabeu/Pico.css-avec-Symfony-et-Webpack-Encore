# Pico.css avec Symfony et Webpack Encore

Proof Of Concept de la mise en place de Pico.css dans un projet Symfony avec Webpack Encore.

Réalisé dans le cadre du tournage d'une vidéo pour la chaine Youtube [YoanDev](https://www.youtube.com/c/yoandevco)

![Gif de présentation](picoCss.gif)

## 1 - Ressources

* [Documentation de Pico.css](https://picocss.com/)
* [Documentation d'installation de Webpack Encore](https://symfony.com/doc/current/frontend/encore/installation.html)

## 2 - Création d'un nouveau projet Symfony

Débutons pas la création d'un nouveau projet Symfony **--full**, et en y ajoutant **webpack encore**. Créons au passage un controller **Home**.

```bash
symfony new picocss --full
composer require symfony/webpack-encore-bundle
npm install
symfony serve -d
symfony console make:controller Home
```

## 3 - Mise en place de Pico.css

Place à l'installation de Pico.css et à l'adaptation de notre application Symfony/Webpack Encore.

- Installons Pico.css avec NPM :

```bash
npm install @picocss/pico
```

- Modification du ```/assets/app.js```

```javascript
/*
 * Welcome to your app's main JavaScript file!
 *
 * We recommend including the built version of this JavaScript file
 * (and its CSS file) in your base layout (base.html.twig).
 */

// any CSS you import will output into a single css file (app.css in this case)
import './styles/app.scss';

// start the Stimulus application
import './bootstrap';
```

- Rennomer ```/assets/styles/app.css``` en ```/assets/styles/app.scss```
- Installer ```npm install sass-loader@^12.0.0 sass --save-dev```
- Décommenter la ligne``` .enableSassLoader()``` dans le fichier ```webpack.config.js```
- Lancer la compilation en mode *watch* : ```npm run watch```
- Remplacer le contenu de ```/assets/styles/app.sccs``` par

```scss
@import "~@picocss/pico/scss/pico.scss";
```

- Modifier le fichier /templates/base.html.twig

```twig
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>{% block title %}Welcome!{% endblock %}</title>
        <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 128 128%22><text y=%221.2em%22 font-size=%2296%22>⚫️</text></svg>">
        {# Run `composer require symfony/webpack-encore-bundle` to start using Symfony UX #}
        {% block stylesheets %}
            {{ encore_entry_link_tags('app') }}
        {% endblock %}

        {% block javascripts %}
            {{ encore_entry_script_tags('app') }}
        {% endblock %}
    </head>
    <body>
        <html id="theme" data-theme="light">
        <main class="container">
            {% block body %}{% endblock %}
        </main>
    </body>
</html>
```

- Et mise en place d'une page de démo dans ```/templates/home/index.html.twig``` pour tester les possibiltés de Pico.css


```twig
{% extends 'base.html.twig' %}

{% block title %}Hello HomeController!
{% endblock %}

{% block body %}
	<nav>
		<ul>
			<li>
				<strong>Symfony Demo Pico.css</strong>
			</li>
		</ul>
		<ul>
			<li><a href="#">Menu 1</a></li>
			<li><a href="#">Menu 2</a></li>
			<li><a href="#">Menu 3</a></li>
            <li><input type="checkbox" id="dark" name="switch" role="switch"></li>
		</ul>
	</nav>

	<div class="grid">
		<div>
			<article>
				Lorem ipsum dolor, sit amet consectetur adipisicing elit. Hic non iure quasi? Est qui facilis vel autem saepe a voluptatem ipsam quo quos eligendi! Nulla ducimus ratione minima corporis eius!
			</article>
		</div>
		<div>
			<article>
				Lorem ipsum dolor, sit amet consectetur adipisicing elit. Hic non iure quasi? Est qui facilis vel autem saepe a voluptatem ipsam quo quos eligendi! Nulla ducimus ratione minima corporis eius!
			</article>
		</div>
	</div>

	<progress id="indeterminate-progress" value="25" max="100"></progress>

    <div class="grid">
        <div>
            <article>
                <form>

                    <!-- Grid -->
                    <div
                        class="grid">

                        <!-- Markup example 1: input is inside label -->
                        <label for="firstname">
                            First name
                            <input type="text" id="firstname" name="firstname" placeholder="First name" required>
                        </label>

                        <label for="lastname">
                            Last name
                            <input type="text" id="lastname" name="lastname" placeholder="Last name" required>
                        </label>

                    </div>

                    <!-- Markup example 2: input is after label -->
                    <label for="email">Email address</label>
                    <input type="email" id="email" name="email" placeholder="Email address" required>
                    <small>We'll never share your email with anyone else.</small>

                    <!-- Button -->
                    <button type="submit">Submit</button>

                </form>
            </article>
        </div>

        <div>
            <article>
                <!-- Select -->
                <label for="fruit">Fruit</label>
                <select id="fruit" required>
                    <option value="" selected>Select a fruit…</option>
                    <option>option1</option>
                    <option>option2</option>
                    <option>option3</option>
                    <option>option4</option>
                </select>

                <!-- Radios -->
                <fieldset>
                    <legend>Size</legend>
                    <label for="small">
                        <input type="radio" id="small" name="size" value="small" checked>
                        Small
                    </label>
                    <label for="medium">
                        <input type="radio" id="medium" name="size" value="medium">
                        Medium
                    </label>
                    <label for="large">
                        <input type="radio" id="large" name="size" value="large">
                        Large
                    </label>
                </fieldset>

                <!-- Checkbox -->
                <fieldset>
                    <label for="terms">
                        <input type="checkbox" id="terms" name="terms">
                        I agree to the Terms and Conditions
                    </label>
                </fieldset>

                <!-- Switch -->
                <fieldset>
                    <label for="switch">
                        <input type="checkbox" id="switch" name="switch" role="switch">
                        Publish on my profile
                    </label>
                </fieldset>
            </article>
        </div>

        <div>
            <article>
                <!-- File browser -->
                <label for="file">File browser
                    <input type="file" id="file" name="file">
                </label>

                <!-- Range slider -->
                <label for="range">Range slider
                    <input type="range" min="0" max="100" value="50" id="range" name="range">
                </label>

                <!-- Date -->
                <label for="date">Date
                    <input type="date" id="date" name="date">
                </label>

                <!-- Time -->
                <label for="time">Time
                    <input type="time" id="time" name="time">
                </label>

                <!-- Color -->
                <label for="color">Color
                    <input type="color" id="color" name="color" value="#0eaaaa">
                </label>
            </article>
        </div>
    </div>

    <article>
        <table>
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Heading</th>
                    <th scope="col">Heading</th>
                    <th scope="col">Heading</th>
                    <th scope="col">Heading</th>
                    <th scope="col">Heading</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th scope="row">1</th>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                </tr>
                <tr>
                    <th scope="row">1</th>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                </tr>
                <tr>
                    <th scope="row">1</th>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                    <td>Cell</td>
                </tr>
            </tbody>
        </table>
    </article>

    <article>
        <a href="#" role="button" class="secondary">Secondary</a>
        <a href="#" role="button" class="contrast">Contrast</a>
        <a href="#" role="button" class="outline">Primary</a>
        <a href="#" role="button" class="secondary outline">Secondary</a>
        <a href="#" role="button" class="contrast outline">Contrast</a>
        <a href='#' role="button" data-tooltip="Tooltip">Tooltip on a button</a>
    </article>

    <article>
        <button aria-busy="true">Please wait…</button>
        <button aria-busy="true" class="secondary"></button>
    </article>
{% endblock %}
```

> Vous pouvez admirer le résultat en consultant la route **/home**, c'est beau n'est-ce pas !?

## 4 - Mettre en place un boutton *Dark/Light Mode*

Par defaut, Pico.css nous offre un thème light et un thème Dark.

Le passage de l'un à l'autre est super simple, il suffit de changer la valeur de la balise ```<html id="theme" data-theme="light">``` par ```<html id="theme" data-theme="dark">``` (que nous avons dans ```/templates/base.html.twig```)

Comme je suis un petit malin, dans le code précedement mis en place, il y'a tout ce qu'il faut, il ne nous manque qu'un bout de JavaScript pour switcher de l'un à l'autre.

- Modifier le fichier ```/assets/app.js``` :

```javascript
/*
 * Welcome to your app's main JavaScript file!
 *
 * We recommend including the built version of this JavaScript file
 * (and its CSS file) in your base layout (base.html.twig).
 */

// any CSS you import will output into a single css file (app.css in this case)
import './styles/app.scss';

// start the Stimulus application
import './bootstrap';

let darkButton = document.querySelector('#dark');
let theme = document.querySelector('#theme');

darkButton.addEventListener('click', () => {
    if (theme.getAttribute('data-theme') === 'light') {
        theme.setAttribute('data-theme', 'dark');
    } else {
        theme.setAttribute('data-theme', 'light');
    }
});
```

## 5 - Customiser les couleurs de Pico.css

Pico.css nous offre la possibilité de customiser simplement les couleurs, et ecore plus facilement puisque dans notre cas nous utilisons scss.

- Modifier le fichier ```/assets/styles/app.scss```

```scss
/* Custom Green version */

// Override default variables
$primary-500: #4caf50;
$primary-600: #43a047;
$primary-700: #388e3c;

@import "~@picocss/pico/scss/pico.scss";
```