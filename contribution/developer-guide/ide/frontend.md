# Frontend

The user-facing part of openVALIDATION-IDE is realized as a single page web application powered by the [angular framework](https://angular.io/). TypeScript is used as the main programming language while HTML and [SCSS](https://sass-lang.com/guide) are utilized for markup and styling respectively.

After you've cloned the [repository](https://github.com/openvalidation/openvalidation-ide) you can install all dependencies through [npm](https://www.npmjs.com/get-npm) using the command`npm install`. Depending on whether you've installed the angular command line tools on your machine globally or not, you can then compile and start the app by running `ng serve` or `npx ng serve`respectively.

## Project structure & architecture

The structure of the application tries to comply with both the official recommendations in regards to [modularization](https://angular.io/guide/module-types) as well as with general best practices regarding scaling angular project architecture. The following figure displays how the general project structure is composed. Each of the areas shown will get further discussed in more detail throughout this section.

![overview of general project structure](../../../.gitbook/assets/image%20%2838%29.png)

### Core Module

First up inside the app folder, we find the core module among others. While the figure only shows the physical file location of the aforementioned modules, the indicated structure is also reflected in the setup of angular modules. The core module is a service module containing and providing singleton services for app-wide cross-cutting concerns like error handling, theme switching, and [backend access](frontend.md#openapi-client-generation).

### Shared Module

In Angular terminology best classified as a widget module, the shared module declares components, directives, and pipes used across the entire application. It is also used to import and then re-export commonly used modules from the Angular core and [Angular Material](https://material.angular.io/) UI component library. Since the shared module itself is used by each feature module, the modules mentioned before do then not have to be explicitly imported again.

### Feature Modules

Structuring code into cohesive chunks of functionality called [feature modules](https://angular.io/guide/feature-modules) is a best practice for angular applications. The feature modules of openVALIDATION-IDE reside inside the modules folder shown in the overview figure from before. Currently the application is made up of four different feature modules. These are: ruleset-editor, ruleset-ide, ruleset-management and ruleset-testsuite.

**ruleset-management:** Concerned with general management and selection of rulesets. It declares two components, one representing an input form for the creation of a ruleset and another one that displays a collection of rulesets to serve as an overview to look at and select rulesets.

**ruleset-editor:** Declares a code editor component with support for openVALIDATION syntax through language server integration for which it provides a corresponding service. For further information about the inner workings of syntax highlighting and linting please refer to the separate section about the [Monaco Editor integration](frontend.md#monaco-editor-integration).

**ruleset-ide:** Expands on the previously mentioned editor module with additional functionality like the visualization of errors, extraction, and display of important variables and the means to manipulate schema attributes. After all, this is the place where everything comes together to provide the user with an integrated development environment for everything rule-set related.

**ruleset-testsuite:** While not being fully implemented just yet, the test-suite module once completed, will provide the means of defining and then automatically validating test cases. For the time being, this functionality is just implied through a visual mockup tho. Nonetheless, a gauge-chart component intended  to illustrate test outcomes is already in a finished state.

### Additional relevant Areas

Referring back to the overview figure from the beginning of this section there are still four additional areas left not mentioned yet. First, there is the **assets** folder currently primarily containing static images for logos, icons but also some additional configuration to fulfill the special requirements of every platform.

Angular allows for CSS rules to be isolated and bound to specific components. This approach is used throughout the App wherever applicable. There is still the need for some global style definitions tho. These reside inside the **styles** folder. Further elaboration on the topic can be found in the section about [theming](frontend.md#theme).

**Environment** files containing static information are a vehicle introduced by angular to provide the ability to define an exchangeable configuration for different build targets. The **config** folder, not an angular default, serves the same purpose with the key difference that it is not consumed by the angular compiler and backed into the build artifacts. Why this is important for the way this application is intended to be deployed and additional information about available configuration options will be discussed in a separate section: [Environmental variables](frontend.md#environmental-variables).

## OpenAPI client generation

Access to the backend is established utilizing an auto-generated backend client. It is created using an [OpenAPI](https://www.openapis.org/) spec file provided by the [backend](backend.md). When the backend modifies its external interface in any meaningful way, a newly updated spec file will reflect these changes. The client code then needs to be re-generated using the new spec. This can be done by simply executing the following command from inside the fronted project:

```text
npm run generate-api-client
```

{% hint style="info" %}
Note that the spec file is hosted by the backend itself, and therefore only accessible when the backend is actively running.
{% endhint %}

The script will then grab the new spec file and feed it, together with some accompanying configuration, into the [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator). The generator is configured to use a specific  [template](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/generators/typescript-angular.md), which will generate an angular module. This module is containing not only angular services ready to be used but also provides strongly typed interfaces for the required parameters and returned models. The aforementioned configuration can be found in a file named _`openapi-generator-config.json`_.

## Theme

Modern and appealing visual design is required to satisfy user expectations. For this reason, an individual design, trimmed for efficiency and ease of use was created especially for the special features of this application. Another important requirement for the application design setup is the capability to serve as a white-label template. Further, the ability to switch between a dark and light mode should be available by default.

With these various requirements in mind, let’s take a look at some implementation details of the current solution. As discussed in an earlier [section](frontend.md#feature-modules), the application is split into various feature modules, each containing one or more sub-components. Most of the custom style definitions are specific to a particular angular component. With angular's characteristic to [scope styles](https://angular.io/guide/component-styles#style-scope) to a component, we gain the advantage not to worry about any potential side effects caused by overlapping style definitions. A maintainable and scalable theming solution should ideally support the capability to influence the visual appearance of the entire application from a single point while keeping the separation of concerns between components regarding individual styling.

The style property we will most likely want to change in between themes is the color palette being used. For this reason, all theme relevant color definitions inside the components are replaced by SCSS variables. With the right tools, autocompletion for SCSS variables will work, but mistakes can still be made. To make the theme system more robust access to theme colors is made through a special function. The function will cause build-time errors when an undefined color variable is used. Each color theme is defined as a [map](https://sass-lang.com/documentation/values/maps), the keys representing the variable, and the value being the assigned color.

{% hint style="warning" %}
Always access theme colors through the **theme-var** mixin e.g.:`background-color: theme-var($--editor-background);`
{% endhint %}

Through the use of SCSS variables, a color palette for a theme can be defined globally, but there is still one crucial disadvantage. When SCSS is compiled into regular CSS, SCSS variables will be replaced by their contents and therefore become static values. Having themes with different color palettes would require separate pre-compiled CSS files. There is an elegant and easy solution, however:

Instead of using SCSS variables directly, each of the SCSS variables in the theme is first defined with a unique native CSS variable as value. When compiled to CSS, the components will now contain CSS variables instead of static values. As CSS variables are evaluated in the browser at runtime, their value is dynamic. To set a CSS variable to the correct value, as defined inside the maps mentioned before, the map's contents need to be converted to CSS classes. This is done by a [mixin](https://sass-lang.com/documentation/at-rules/mixin), which is called once for each theme map. Changing the theme is now as easy as swapping out a single class. This way, the benefits of using SCSS variables can be retained while also providing the capability to change color palettes dynamically.

{% tabs %}
{% tab title="scss to css variable" %}
```css
// _variables.scss
$--navbar-background: --navbar-background;
```
{% endtab %}

{% tab title="map to class" %}
```text
// _mixins.scss
@mixin spread-map($map: ()) {
    @each $key, $value in $map {
        #{$key}: $value;
    }
}

// themes.scss
:root.light-theme {
    @include spread-map($light-theme)
}
```
{% endtab %}
{% endtabs %}

Finally, to provide the means of changing the theme programmatically in angular, there is a service called theme-service. The service exposes the functionality to transition between themes and provides the ability to query whether a dark or light theme is currently active.

## Monaco Editor integration

To edit the rulesets created and managed by the IDE the [Monaco Editor](https://microsoft.github.io/monaco-editor/) is used. It has support for a lot of useful features like linting, auto-completion and syntax highlighting and is integrated into the project using the [ngx-monaco-editor](https://github.com/atularen/ngx-monaco-editor) Angular Component. Because the openVALIDATION language is custom, it needs a custom [language-server](https://github.com/openvalidation/openvalidation-languageserver) implementation to handle the above mentioned features. The code to integrate the custom language-server is contained in its own service. The language-server integration uses the [TypeFox monaco-languageclient](https://github.com/TypeFox/monaco-languageclient) library and was inspired by Niko Lueg's initial [client implementation](https://github.com/NLueg/monaco-ov) for the openvalidation-languageserver. This also includes the tokenizer that was only adapted to work with Angular and styled according to the projects linting guidelines.

### Syntax highlighting

Most of the syntax-highlighting features are implemented in the language-server. In order to improve the usability some features were added to the already included syntax-highlighting features. These include:

* Highlighting schema attributes in their respective type color
* Coloring the error messages after "then" in the appropriate error color
* Marking boolean values in their respective color
* Fixing syntax-highlighting errors when words include other keywords

## Environmental variables

It is possible to configure the backend and language-server URLs using environment variables. These can be set in the docker-compose file or when the container is started.

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| API\_BASE\_PATH | http://127.0.0.1:8080 | URL of the IDE Backend |
| LANGUAGE\_SERVER\_URL | ws://127.0.0.1:3010 | URL of the language-server |

In a default, angular application environmental variables can be set through the means of angular's predefined environment files. As mentioned earlier, these files are baked into the final artifacts when the application is built and hence cannot be altered after the fact. Docker environmental variables are used to customize already existing container images and therefore come into effect when the angular environment variables are already set and cannot be accessed anymore.

To solve this problem values in the angular environment files are set to a global variable inside the window object e.g. `window['env']['API_BASE_PATH']`. These variables are set by a script that is loaded in the _`index.html`_ before angular is loaded. The default version of the script contains all the default values illustrated in the table above. Because the script is defined as a static asset of the angular application, it will not be baked into the application's code.

Alongside the script containing the default values, which is named _`env.js`_ and to be found inside the conf folder, there is a template version called _`env.template.js`_ . The template version contains all the same definitions but with they're values set to a placeholder looking like this `"${API_BASE_PATH}"`.

When the container is started, it takes the template version, replaces all variables with corresponding docker environment variables, and then finally replaces the default _`env.js`_ with this new values.

