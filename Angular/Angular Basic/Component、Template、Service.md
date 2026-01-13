

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

對於我們自己自訂的 component，我們也會希望像是其他 html element 一樣具備一些屬性能夠輸入，而 `@Input` 就是達成這個方式的做法。當在自訂義的 compoenent 中使用 `@Input`，就能夠讓外部其他元件在使用時透過  property binding 的方式綁定到 component 中去依照輸入內容執行自訂義的邏輯。

下面的做法是透過 input signal 的方式去為 component 綁定新的屬性，可以透過 `required` 來確保屬性必須被填入，對於一些版本較舊的 angular 專案，實際上可能會使用 `@Input` decorator 來使用，但概念上是相同的。

兩種用法可以參考官方文件
`input()` -  [input • Angular](https://angular.dev/api/core/input)
`@Input()` - [Accepting data with input properties • Angular](https://angular.dev/guide/components/inputs#declaring-inputs-with-the-input-decorator)  

```ts
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-tasks',
  standalone: true,
  imports: [],
  templateUrl: './tasks.component.html', styleUrl: './tasks.component.css'
})
export class TasksComponent {
  name = input.required<string>();
}

```

```ts
<main>
  //....
  <app-tasks [name]="selectedUserName"></app-tasks>
</main>

```


## @Output vs output()

Output 的概念與 Input 類似，其實就是讓我們自訂義的 component 能夠去發出一些客製化的事件供其他元件去使用，同樣也是提供 output signal 與 @Output 做使用，以下面的例子來說，`UserComponent` 會透過 emit 去發出值，而在上層 `AppComponent` 的 template 中，就能夠透過監聽這個客製的 `selected` 去執行對應的操作，這裏用到的 `$event` 是 angular 中一個特別的變數，說明於 [Event](##Event)

```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'app-user',
  standalone: true,
  templateUrl: './user.component.html',
  styleUrl: './user.component.css',
})
export class UserComponent {
  @Input({ required: true }) user!: {
    id: string;
    name: string;
    avatar: string;
  };

  @Output() selected = new EventEmitter<string>();

  get imagePath() {
    return 'assets/users/' + this.user.avatar;
  }

  onSelectUser() {
    this.selected.emit(this.user.id);
  }
}

```

```ts
<app-header />

<main>
  <ul id="users">
    <li>
      <app-user
        [user]="users[0]"
        (selected)="onUserSelected($event)"
      />
    </li>
  //....
</main>
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


## Type Alias && Interface


### Attribute vs Propetry

[HTML attributes vs DOM properties - JakeArchibald.com](https://jakearchibald.com/2024/attributes-vs-properties/)


## 檔案分類

通常會將自定義的 object 以 `xxx.model.ts` 的方式放到額外的檔案中進行分類

```ts
//task.model.ts
export interface task {
  id: string;
  userId: string;
  title: string;
  summary: string;
  dueDate: string;
}
```


並在要使用的 component 中引入。
而這時如果很明確引入的會是一個型別，可以在前方加上 `type` 增加可讀性
```ts
import { type task } from './task.model'
```

## CSS 動態綁定

當想要針對特定的元素進行動態時，可以透過 style binding 的方式

以下面方式來說，有一個 `.active` 的 css 檔案，如果希望能夠在特定條件下使用它時，就可以透過 `[class.yourclassname]` 的方式動態綁定 css，後面會接上一個 boolean 變數，當他會 `true` 時就會套用該樣式，反之則不會套用。

```css
.active {
  background-color: #9965dd;
  color: #150722;
}
```

```ts
<!-- app.component.html -->
<div [class.active]="isActive">
  This div's class depends on the 'isActive' property.
</div>

```

[Angular - Class and style binding](https://v17.angular.io/guide/class-binding#binding-to-a-single-css-class)