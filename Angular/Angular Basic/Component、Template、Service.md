

## 綁定動態資料的兩種方式

- `string interpolation` - 利用雙括號來包含要顯示的資料

```html
<p>Hello, {{ name }}!</p>
```

- `property binding` - 透過`[]`屬性綁定的方式來將值綁定到元素上

```html
<div>
  <button (click)="onSelectUser()">
    <img [src]="imagePath" [alt]="selectedUser.name" />
    <span>{{ selectedUser.name }}</span>
  </button>
</div>

```



## Event

- `$event`
	- event 參數通常會在觸發事件後，將它作為參數
	- https://angular.dev/guide/templates/event-listeners#accessing-the-event-argument


### Attribute vs Propetry

[HTML attributes vs DOM properties - JakeArchibald.com](https://jakearchibald.com/2024/attributes-vs-properties/)