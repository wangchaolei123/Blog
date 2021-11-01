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