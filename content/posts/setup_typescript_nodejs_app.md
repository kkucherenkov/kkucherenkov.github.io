---
title: "Настройка NodeJS проекта с TypeScript"
description: "Пошаговая настройка Node.js-проекта с TypeScript: компилятор, линтер, структура и скрипты сборки. (перевод)"
date: 2023-12-12T17:54:16+04:00
draft: false
toc: false
images:
tags:
  - translation
  - typescript
  - nodejs
---

В этом гайде мы пройдем через процесс создания TypeScript проекта с нуля. В результате мы настроим холодную перезагрузку и сборочные скрипты для `production` и `development` окружений.

## Предварительные условия
- У вас должны быть установлены NodeJS и npm
- У вас должно быть базовое понимание NodeJS и экосистемы npm
- У вас должен быть установлен редактор кода, например VS Code

## Первоначальная установка
Давайте создадим директорию в которой будем работать

```bash
mkdir typescript-starter
cd typescript-starter
```

Далее нам нужно настроить проект, добавить файл `package.json` и добавить зависимости

### Настройка `package.json`
```bash
npm init -y
```
Если использовать флаг `-y` при создании проекта, то мы автоматически согласимся со всеми настройками "по умолчанию"

```bash
npm install typescript --save-dev
```
Теперь нужно настроить TypeScript

### Установка глобальных типов TypeScript для NodeJS
В TypeScript есть явные (explicit), неявные (implicit) и глобальные (global) типы. 
Неявные типы вычисляются TypeScript на основе начального значения переменной и выглядят так же, как обычный JavaScript.
Явные типы — это когда мы намеренно предоставляем тип, используя специальный синтаксис TypeScript.
Глобальные типы добавляются в общую область исполнения. Поскольку мы используем Node, было бы хорошо, если бы мы могли обеспечить безопасность типов и автодополнение в API Node, например `file`, `path`, `process` и т. д. Для этого мы установим определения типов для NodeJS

```bash
npm install @types/node --save-dev
```

### Настраиваем  `tsconfig.json`.
Файл  `tsconfig.json` содержит настройки компилятора TypeScript. Создадим этот файл с несколькими заданными опциями:
```bash
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

-  `rootDir`: Это путь, по которому TypeScript будет искать наши исходные файлы. Мы задали директорию `src/`, в ней мы будем писать наш код.
- `outDir`: Это путь, по которому TypeScript будет складывать скомпилированный код. Мы указали директорию `build/`.
- `esModuleInterop`: Если вы были в сфере JavaScript в течение последних нескольких лет, вы, возможно, заметили, что системы модулей немного вышли из-под контроля (AMD, SystemJS, ES Modules и т. д.). Если мы используем commonjs в качестве нашей системы модулей (для приложений Node вы должны это делать), тогда нам нужно установить для этого параметра значение true.
- `resolveJsonModule`: Если в проекте есть json файлы, эта опция позволит TypeScript ипользовать их.
- `lib`: Эта опция добавляет в наш проект глобальные типы, что позволяет нам использовать функции из разных версий Ecmascript, библиотек для тестирования и даже API DOM браузера. Мы хотели бы использовать некоторые возможности языка es6. Все это компилируется в es5.
- `module`: `commonjs` это стандартная система модулей Node в 2019. 
- `allowJs`: если вы переводите свой старый JavaScript проект в  TypeScript, эта опция позволит вам использовать `.js` файлы вместе с  `.ts` файлами.
- `noImplicitAny`: запрет неявного указания типа в файлах TypeScript . Каждый тип должен либо иметь определенный тип, либо быть явно объявлен «any». Никаких неявных «any».

На данный момент наш файл `tsconfig.json` выглядит примерно так:
```json
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    "lib": ["es6"],                     /* Specify library files to be included in the compilation. */
    "allowJs": true,                          /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "build",                          /* Redirect output structure to the directory. */
    "rootDir": "src",                         /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                    /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "resolveJsonModule": true                 /* Include modules imported with '.json' extension */
  }
}
```

Можно пойти дальше и почистить его от ненужных и закоментированных строк:

```json
{
  "compilerOptions": {
    "target": "es5",                          
    "module": "commonjs",                    
    "lib": ["es6"],                     
    "allowJs": true,
    "outDir": "build",                          
    "rootDir": "src",
    "strict": true,         
    "noImplicitAny": true,
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
```

Мы готовы написать и запустить наш первый TypeScript файл

### Создание `src` и первой программы на TypeScript

```bash
mkdir src
touch src/index.ts
```

Теперь добавим немного кода:
```typescript
console.log('Hello world!')
```

### Компиляция нашего TypeScript
Чтобы скомпилировать наш код, нам нужно запустить команду `tsc` с помощью `npx`, исполнителя пакетов Node. `tsc` прочитает `tsconfig.json` в текущем каталоге и применит конфигурацию к компилятору TypeScript для создания скомпилированного кода JavaScript.

```bash
npx tsc
```

Проверим, что получилось после компиляции в файле  `build/index.js`.

```javascript
"use strict";
console.log('Hello world!');
```

## Полезные настройки и скрипты
### Холодная перезагрузка

Холодная перезагрузка полезна при локальной разработке. Для её настройки нам понадобится еще пара пакетов: `ts-node` для запуска кода TypeScript напрямую, не дожидаясь его компиляции, и `nodemon` для отслеживания изменений в нашем коде и автоматического перезапуска при изменении файла.

```bash
npm install --save-dev ts-node nodemon
```

Добавим конфигурационный файл для nodemon: `nodemon.json`.

```json
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "npx ts-node ./src/index.ts"
}
```

Для того, чтобы воспользоваться преимуществами nodemon добавим его вызов при запуске нашего приложения в `package.json`

```json
"start:dev": "npx nodemon",
```

Теперь, при запуске команды `npm run start:dev`, `npx nodemon` запустит наше приложение с помощью `npx ts-node ./src/index.ts` и будет следить за изменениями  в файлах `.ts` и `.js` в директории `/src`.

### Создание релизных сборок

Для того, чтобы сделать чистую компиляцию проекта для релиза мы можем добавить скрипт `build`

Установим пакет `rimraf`, независящую от платформы команду, действующую как  `rm -rf` (просто удаляет то, что вы её скажете)

```bash
npm install --save-dev rimraf
```

И после этого добавьте в файл `package.json`

```json
"build": "rimraf ./build && tsc",
```

Теперь, когда мы выполним `npm run build`, `rimraf` очистит нашу старую директорию `build` до того, как компилятор TypeScript создаст новый код в `build`.

### Запуск релизной сборки
Для того, чтобы запустить релизную сборку нам нужно сначала выполнить команду `build` а затем запустить скомпилированный файл `build/index.js`.

Скрипт запуска выглядит так (`package.json`):
```json
"start": "npm run build && node build/index.js"
```

---

## Обзор скриптов
### `npm run start:dev`
Запускает приложение в режиме разработки с помощью `nodemon` и  `ts-node`, поддерживает перезапуск при изменении файлов.

### `npm run build`
Собирает приложение в  `build`, предварительно очищая директорию сборки.

### `npm run start`
Запускает приложение в релизном режиме.
