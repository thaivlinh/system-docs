---
sidebar_position: 2
---

## .env

Create an `.env` file in root folder

```
...
APP_SECRET=123123
...
```

## Config folder

Create a `config` folder in ther `src` folder. It will manage all system wide configurations, including environment variables.

### Interface

- Define all configuration typing here
- Define enum of all .env config keys

### Factory

- Use dotenv to load .env config

  ```ts
  import * as dotenv from 'dotenv'; 
  dotenv.config();
  ```
  
- Import some metadata from package.json
  
  :::caution 
  
  Add the following option to tsconfig.json
  ```
  "resolveJsonModule": true,
  ```
  :::
- Export a config function that return all configs

### service

- Create a custom ConfigService instead of default ConfigService from `@nestjs/config`
  ```ts
  import { ConfigService as NestConfigService } from '@nestjs/config';

  export class ConfigService {
    constructor(
      @Inject(NestConfigService)
      private readonly configService: NestConfigService,
    ) {}
  }
  ```
  
- Manually add new get method for new config group, so the typing can work

### Module

- Import **ConfigModule** and flag as global 
- Load config from the factory by `load` instead of using `envFilePath`
```ts
import { ConfigModule as NestConfigModule } from '@nestjs/config';

///

imports: [
    NestConfigModule.forRoot({
      isGlobal: true,
      cache: true,
      load: [configFactory],
    }),
  ],
```
