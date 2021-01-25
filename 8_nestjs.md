## JSON Web Token
- 오픈 소스 산업 표준 (RFC-7519)
- 허가 또는 당사자 간의 안전한 정보 교환에 사용
- 발신인이 자신이 누구인지 확인
- 발급자가 비밀 또는 키 쌍(HMAC 알고리즘, RSA 또는 ECDSA)을 사용하여 서명

---
- Header: 토큰에 대한 메타데이터 포함 (type, hashing algorithm)
- Payload: 클레임(예: 사용자) 및 추가 데이터가 포함
- Signature: 암호로 서명된 인코딩된 헤더의 결과

---
- 아무나 해독할 수 있는건 아니지만 패스워드같은 민감한 정보를 포함해서는 안됨

### passportJS
- yarn add @nestjs/jwt @nestjs/passport passport passport-jwt

```ts
// auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { TypeOrmModule } from '@nestjs/typeorm';
import { AuthController } from './auth.controller';
import { AuthService } from './auth.service';
import { UserRepository } from './user.repository';
import { PassportModule } from '@nestjs/passport';

@Module({
  imports: [
    PassportModule.register({ defaultStrategy: 'jwt' }),
    JwtModule.register({
      secret: 'topSecret51',
      signOptions: {
        expiresIn: 3600,
      },
    }),
    TypeOrmModule.forFeature([UserRepository]),
  ],
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```