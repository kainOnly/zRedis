## 配置 (ConfigService)

#### originUrl: string

RESTful Api 请求接口的域名，例如 `https://api.developer.com`

#### staticUrl: string

静态资源地址，可以是 `origin` 域名的相对路径，也可以是cdn域名，例如，`https://cdn.developer.com/`

#### iconUrl: string

放置在CDN上的icons路径，例如，`https://cdn.developer/icons/`

#### namespace: string

RESTful Api 地址命名空间，例如，`sys`，如果没有请设置为 `''`

#### uploadsUrl: boolean

是否为分布上传，`false` 为 `originUrl`+`/`+`uploadsPath`，`true` 时 `uploadsPath` 需填写完整上传地址

#### uploadsPath: string

上传地址

#### withCredentials: boolean

允许请求携带Cookie，设置为 `true`

#### httpInterceptor: boolean

是否开启请求拦截

#### interceptor = (res) => of(res)

请求拦截自定义处理，例如对RBAC返回失败的统一请求进行拦截并返回提示

```typescript
import {Component, OnInit} from '@angular/core';
import {BitService, ConfigService} from 'ngx-bit';
import {Observable, of} from 'rxjs';
import {NzMessageService} from 'ng-zorro-antd';
import packer from './app.language';

@Component({
  selector: 'app-root',
  template: '<router-outlet></router-outlet>',
})
export class AppComponent implements OnInit {
  constructor(private bit: BitService,
              private message: NzMessageService,
              private config: ConfigService) {
  }

  ngOnInit() {
    this.bit.registerLocales(packer, true);
    this.config.interceptor = (res: any): Observable<any> => {
      if (res.error && res.msg === 'error:rbac') {
        this.message.error(this.bit.l.rbac_error);
      }
      return of(res);
    };
  }
}
```

#### breadcrumbTop: any

面包屑默认最高级，默认 `0`

#### pageLimit: number

列表分页, 默认值 `20`

#### formControlCol: any

表单 `Control` 栅格统一设置

- common: any，表单公共栅格标识
- submit: any，表单提交栅格标识

```typescript
formControlCol: {
  common: {
    nzMd: 10,
    nzSm: 12,
    nzXs: 24
  },
  submit: {
    nzMd: {span: 10, offset: 7},
    nzSm: {span: 12, offset: 7},
    nzXs: {span: 24, offset: 0}
  }
}
```

#### formLabelCol: any

表单 `Label` 栅格统一设置

```typescript
formLabelCol: {
  common: {
    nzSm: 7,
    nzXs: 24
  },
}
```

#### i18nDefault: string

多语言输入组件默认标识，默认 `zh_cn`

#### i18nContain: any[]

多语言输入组件标识数组，例如：设置中文与英文，`['zh_cn', 'en_us']`

#### i18nSwitch: any[]

多语言组件集合，`i18n` 需要于标识对应

```typescript
[
    {
        i18n: 'zh_cn',
        name: {
            zh_cn: '中文',
            en_us: 'Chinese'
        }
    },
    {
        i18n: 'en_us',
        name: {
            zh_cn: '英文',
            en_us: 'English'
        }
    }
]
```

