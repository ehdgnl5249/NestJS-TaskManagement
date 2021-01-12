# Object Relational Mapping (ORM) and TypeORM
- ORM(객체 관계 매핑)은 객체 지향 패러다임을 사용하여 DB의 데이터 쿼리를 조작할 수 있는 기술
- 개발자가 일반 쿼리를 직접 보내는 것보다 각자 선호하는 프로그래밍 언어를 사용하여 DB와 통신할 수 있는 ORM 라이브러리가 많음

## ORM의 장점
- 데이터 모델을 한 곳에 기록해서 유지 관리가 더 쉬워짐. 반복이 적음.
- 많은 것들이 자동으로 수행됨 (데이터 핸들링, 타입, 관계 등)
- SQL 구문을 작성할 필요 없음. 코딩으로 해결
- 데이터베이스 추상화 (원할 때 DB 타입 변경 가능)
- OOP를 활용하기 때문에 쉽게 상속 가능

## ORM의 단점
- ORM 라이브러리는 항상 단순한 것은 아님
- 다양한 유지 관리 문제로 이어질 수 있는 ORM 뒤에서 일어나고 있는 일을 무시하게 될 수 있음

## TypeORM
- Typescript에서 사용되는 ORM 라이브러리
- 엔티티, 저장소, 커럼, 관계, 복제, 쿼리, 로깅 등을 정의하고 관리하는 데 도움을 줌.

- * Ashley의 작업 중, Done 상태인 작업만 가져오는 ORM 코드는
```ts
const tasks = await Task.find({ user: 'Ashely', status: 'Done' });
```
- 순수 자바스크립트로 짠다면 db.query(~~~~~~~~) 로 직접 SQL문을 사용하며 처리해야하는 복잡함이 있음
- https://typeorm.io (TypeORM 공식 문서)
- yarn add @nestjs/typeorm typeorm pg

---
- /src/config/typeorm.config.ts 생성
```ts
import { TypeOrmModuleOptions } from '@nestjs/typeorm';

export const typeOrmConfig: TypeOrmModuleOptions = {
  type: 'postgres',
  host: 'localhost',
  port: 5432,
  username: 'postgres',
  password: '1234',
  database: 'taskmanagement',
  entities: [__dirname + '/../**/*.entity.{js,ts}'],
  synchronize: true,
};
```

- typeORM을 사용하기 위해서 app.module.ts 에서 import 하기
```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { typeOrmConfig } from './config/typeorm.config';
import { TasksModule } from './tasks/tasks.module';

@Module({
  imports: [
    TypeOrmModule.forRoot(typeOrmConfig), 
    TasksModule,
  ],
})
export class AppModule {}
```