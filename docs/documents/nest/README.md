## 路由指向
初始化，`src`中`main.ts`代码如下
```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
` await NestFactory.create(AppModule);`表示使用nest的函数创建`AppModule`,监听在3000端口
`src`中的`app.service.ts`，代码如下
```ts
import { Injectable } from '@nestjs/common';
@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}

```
`AppService`类中的`getHello`的返回`Hello World!`。在 `src` 下的 `app.controller.ts`被调用。
- ` @Get('hello-world')` 表示接口为`/hello-world`,`get`请求
## 路由前缀
- ### 局部路由前缀
在`app.controller.ts`,在`@Controller()`写入`example`,表示当前文件中，所有路由都有前缀`exmaple`，请求`example/hello-world`，调用`getHello()`方法
- ### 全局路由前缀
只需要在 `main.ts` 中加上`app.setGlobalPrefix()`
```ts
 app.setGlobalPrefix('nest');
```

## 使用Swagger

nestjs对于swagger的集成非常友好，需要下载相关的swagger的node包`npm i --save @nestjs/swagger swagger-ui-express `

- 在`main.ts`中 

  ```ts
  import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger'
  async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    const options = new DocumentBuilder()
      .setTitle('标题')  //标题
      .setVersion('nestjs')  //描述
      .setDescription('1.0')//版本
      .build()
    const documnet = SwaggerModule.createDocument(app, options)
    SwaggerModule.setup('api', app, documnet);
    await app.listen(8000);
  }
  ```

- 接口文档

  在每个控制器文件中添加

  ```ts
  @ApiTags('帖子')//为每个控制的标签
  @ApiOperation({ summary: '创建帖' })//每个接口的描述
  ```

  参数描述

  ```ts
  //通过接口和 ApiProperty来描述接口的参数
  class CreatPost {
      @ApiProperty({ description: '标题' })
      title: string;
      @ApiProperty({ description: '内容' })
      content: string;
  }
   @Post()
    @ApiOperation({ summary: '创建' })
     create(@Body() body: CreatPost) {
  		return body
   }
  //通过ApiParam，ApiQuery，ApiHeader装饰器，required为true，参数必传
  @ApiParam({ description: '用户id', name: 'id' })
  ```

  