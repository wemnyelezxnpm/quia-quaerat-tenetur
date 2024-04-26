# @wemnyelezxnpm/quia-quaerat-tenetur
[![npm version](https://badge.fury.io/js/@wemnyelezxnpm/quia-quaerat-tenetur.svg)](https://badge.fury.io/js/@wemnyelezxnpm/quia-quaerat-tenetur)
[![Language grade: JavaScript](https://img.shields.io/lgtm/grade/javascript/g/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur/context:javascript)
[![Build Status](https://travis-ci.org/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur.svg?branch=master)](https://travis-ci.org/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur)
[![Coverage Status](https://coveralls.io/repos/github/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur/badge.svg?branch=master)](https://coveralls.io/github/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur?branch=master)
[![dependencies Status](https://david-dm.org/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur/status.svg)](https://david-dm.org/bpatrik/@wemnyelezxnpm/quia-quaerat-tenetur)
 
Configuration file for Typescript.
Useful for separating frontend (public) and backend (private) config
Features:
 - Creating Configuration in Typescript file with types
 - Loading configuration from file, command line arguments, environmental variables
 - Support config file hierarchy
 
#Install

```shell
npm install @wemnyelezxnpm/quia-quaerat-tenetur
```

#Usage

```typescript
  @SubConfigClass()
      class S {

        @ConfigProperty({envAlias: 'numAlias'})
        num: number = 5;

        @ConfigProperty({type: 'ratio',
          onNewValue: (v, c: C) => {
            c.temperature=v*100;
          }})
        temperatureRatio: number = 0.2;

      }

      @ConfigClass()
      class C {


        @ConfigProperty()
        sub: S = new S();

        @ConfigProperty({type: 'integer'})
        num: number = 5;

        @ConfigProperty({type: 'integer', constraint: {assert: v => v < 100 && v >0}})
        temperature: number = 5;


      }
```

# Legacy usage

Legacy usage is still supported and can be accessed like the following way:

### backend
```typescript
let Config = {
    Private:{
        something:5,
        PORT:1234
    },
    Public:{
        a:6
    }
};
 

ConfigLoader.loadBackendConfig(Config, //Config object to load the data to
    path.join(__dirname, './../../../config.json'), // configuration file path
    [["PORT", "Private-PORT"]]); //environmental variable mapping to config variable
```

### frontend

```typescript
let Config = {
    Public:{
        a:6
    }
};

if (typeof ServerInject !== "undefined" && typeof ServerInject.ConfigInject !== "undefined") {
    WebConfigLoader.loadFrontendConfig(Config.Public, ServerInject.ConfigInject);
}
```

### config changing
  * updating config file (if not exist, it will be created)
  * setting environmental variable
  * Command line arguments
    *  `node index.js --Private-something=3 --Public-a=10`


# Recommended Usage
See `example/legacy` folder.

The architecture helps separating public and private configuration. Private config will be only available at server side, while Public config at front and backend side too.
The up-to-date public config is sent to the frontend with ejs template.
