

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

$event 是 angular 中一個特殊的參數，event 參數通常會在觸發事件後，將它作為參數丟入 callback function 中，而 $event 其實就是前面觸發的事件發出的詳細訊息。以 `keyEvent` 來說，可以從中取得是哪個 key 被按下了。

```ts
@Component({
  template: `
    <input type="text" (keyup)="updateField($event)" />
  `,
  ...
})
export class AppComponent {
  updateField(event: KeyboardEvent): void {
    console.log(`The user pressed: ${event.key}`);
  }
}
```

對於自訂義的 event 來說，狀況也是相同的。下方的程式碼中的 `selectPlace` 發出了一個訊號 `Place`，而在 template 中，也透過 `$event` 傳遞給 callback function `onSelectedUserPlace`，這時這個 function 取得的東西其實就是 `Place`

```ts
export class PlacesComponent {
  places = input.required<Place[]>();
  selectPlace = output<Place>();

  onSelectPlace(place: Place) {
    this.selectPlace.emit(place);
  }
}
```

```ts
  @if (places()) {
    <app-places [places]="places()!" (selectPlace)="onSelctedUserPlace($event)"/>
  } @else if (places()?.length === 0) {
    <p class="fallback-text">Unfortunately, no places could be found.</p>
  }
```



- https://angular.dev/guide/templates/event-listeners#accessing-the-event-argument

## Getter

- [ ] getter


## !?


- ! - 宣告時代表該變數必定不為空值或是 null
- ? - 代表是可選的變數

```ts

//宣告一必定不為空的變數

name!:string;

//在 function 中作為可選變數

function func(name?:string) {
	//....
}

```


## @Input vs input() 


## @Output vs output()




### Attribute vs Propetry

[HTML attributes vs DOM properties - JakeArchibald.com](https://jakearchibald.com/2024/attributes-vs-properties/)