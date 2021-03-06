## 搜索触发

#### @Directive({selector: '[bitSearchStart]'})

```typescript
@Directive({
  selector: '[bitSearchStart]'
})
export class BitSearchStartDirective {
  @Input() bitSearchStart: string;
  @Output() after: EventEmitter<any> = new EventEmitter<any>();

  constructor(private bit: BitService,
              private storage: LocalStorage) {
  }

  @HostListener('keydown.enter') onenter() {
    this.searchStart();
  }

  @HostListener('click') onclick() {
    this.searchStart();
  }

  /**
   * search data save storage
   */
  private searchStart() {
    this.storage.setItem('search:' + this.bitSearchStart, this.bit.search).subscribe(() => {
      this.after.emit(true);
    });
  }
}
```

- **@Input() bitSearchStart: string** 搜索命名
- **@Output() after: EventEmitter< any >** 开始搜索之后

注册搜索字段

```typescript
 this.bit.registerSearch('app-index', {
  field: 'name', op: 'like', value: ''
}).subscribe(() => {
  ...
});
```

同时给组件加入 `click` 与 `enter` 触发搜索

```html
<nz-input-group nzSearch [nzAddOnAfter]="nzAddOnAfter">
  <input type="text" [(ngModel)]="bit.search[0].value"
          bitSearchStart="app-index"
          (after)="getLists(true)"
          nz-input [placeholder]="bit.l['search']">
</nz-input-group>

<ng-template #nzAddOnAfter>
  <button nz-button nzType="primary" nzSearch
          bitSearchStart="app-index"
          (after)="getLists(true)">
    <i nz-icon type="search"></i>
  </button>
</ng-template>
```
