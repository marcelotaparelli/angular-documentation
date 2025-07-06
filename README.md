# angular-documentation

## Start

### - ng new [name-of-your-project]

### - ng serve 
precisa rodar dentro da página do projeto criado

### - ng generate component  /  ng generate interface  /  ng generate service
ng g c  /  ng g i  /  ng g s
nome-da-pasta/nome-do-componente

<br><br>

## Component Structure

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-container',
  imports: [],
  templateUrl: './container.component.html',
  styleUrl: './container.component.css'
})

export class ContainerComponent {

}
```

<br><br>

## Para importar um componente

```
import { NomeDoComponente } from './nomeDaSuaPasta/seu-componente.component';
...
imports: [NomeDoComponente]',
...
```

<br><br>

## Para aceitar conteúdo dentro da tag do componente

```<ng-content />```

<br><br>

## Interpolation
```{{ yourVar }}```

<br><br>

## Property binding
```<img [src]="imageURL">```

now, src is bound to the var imageURL

<br><br>

## Event binding
```<buton (onclick)="yourFunc()">```

<br><br>

## Two-way data binding (banana() in a box[])

```<input [(ngModel)]= "yourProperty">```

<br><br>

## Event handling
```<button (click)="yourFunction()">```

define yourFunction in the class definition

<br><br>

## Para receber propriedades do elemento pai -> Input()
```
//na classe filha
suaVar = input<seuTipo>();

//no template filho
{{ suaVar() }}

na classe pai
<app-componente-filha suaVar="Ola" />

```

<br><br>

## Para emitir eventos -> Output()

1. registre um evento com output() e o tipo emitido
2. gere uma função para emitir o evento com .emit() tendo como parametro o dado que quer emitir com o tipo definido em output()
3. associe a função a algum elemento no template da filha
4. no elemento pai adicione a tag da filha com o evento entre parenteses e coloque a função que vai receber o dado do evento da filha
5. defina a função no pai que vai receber o dado do evento
```
//na classe filha
yourEvent = output<seuTipo>();

yourFunction() {
  // adicione a lógica do evento
  this.yourEvent.emit(seuTipoAlterado)
}

//no template da filha
<button (click)="yourFunction()">


//no template pai
<app-componente-filha (yourEvent)="handleEvent($event)" />

//na classe pai
handleEvent(arg: seuTipo){
  sua lógica
}
```

Fluxo:  botão ativa yourFunction() na filha que ativa emit(), evento é capturado pela função handleEvent() no pai 

<br><br>

## Diretivas

### @if () {} @else {}

### @for (item of yourItems; track item.id) {}

<br><br>

## Para carregar conteúdos pesados qundo usuário chegar no viewport

Placeholder: é o que fica até o usuario chegar ao viewport
Loading: o que aparece enquanto está carregando
Defer: o que vai ser carregado depois

```
@defer (on viewport) {
  <comments />
} @placeholder {
  <p>Future comments</p>
} @loading (minimum 2s) {
  <p>Loading comments...</p>
}
```

<br><br>
<br><br>

## Formulários

https://angular.dev/guide/forms

**Reactive Forms:**

Crie o FormGroup no seu component

```
import { FormGroup, FormControl, ReactiveFormsModule, Validators } from '@angular/forms';
...
imports: [...
          ReactiveFormsModule],

//na sua classe
yourForm!: FormGroup;

constructor() {
  this.yourForm = new FormGroup({
    nome: new FormControl(valorInicial, Validators.required);
    email: new FormControl('', [Validators.required, Validators.email]);
    ...
  })
}
```

Associe a um form no template

```
<form [formGroup]="yourForm" (ngSubmit)="suaFuncaoDeHandleSubmit()">

suaFuncaoDeHandleSubmit() {
 if (this.yourForm.valid) {
    //your logic
 }
}
```

Associe cada campo a um FormControl

```<input ... formControlName="nomeDefinidoNoFormGroup"```

Acesse os valores do form na sua classe do componente com

```
this.yourForm.value
this.yourForm.value.nome
```

Trabalhe as exceções dos Validators

```
//adicione no template
@if (contatoForm.get('nome')?.errors && contatoForm.get('nome')?.touched) {
<div class="mensage-erro"> Campo obrigatório </div>
}
```

Trabalhe as exceções específicas

```
//adicione no template
@if (contatoForm.get('nome')?.errors['required'] && contatoForm.get('nome')?.touched) {
<div class="mensage-erro"> Campo obrigatório </div>
}
@if (contatoForm.get('nome')?.errors['email'] && contatoForm.get('nome')?.touched) {
<div class="mensage-erro">Email inválido</div>
}
```

Invalide o botão de submit

```
<button [disabled]="yourForm.invalid"
        [ngClass]="yourForm.valid ? 'botão-habilitado' : 'botao-desabilitado'"   //definir as classes
        type="submit">Enviar</button>
```

<br><br>

Para resetar o formulário

```
this.yourForm.reset();
```

## Router

Faça a importação
```
import { RouterOutlet } from '@angular/router';
...
imports: [...
         RouterOutlet],
```

No app.config.ts
```
import {ApplicationConfig} from '@angular/core';
import {provideRouter} from '@angular/router';
import {routes} from './app.routes';

export const appConfig: ApplicationConfig = {
providers: [provideRouter(routes)],
};
```

Adicione no app.component.html
```
import {RouterOutlet} from '@angular/router';
@Component({
...
template: `     <nav>
      <a href="/">Home</a>
      |
      <a href="/user">User</a>
    </nav>
    <router-outlet />
  `,
imports: [RouterOutlet],
})
export class App {}
```

app.routes.ts
```
import { Routes } from '@angular/router';
import { YourComponents } from './your-paths';

export const routes: Routes = [
  {
    path: 'your-url',
    component: YourComponent
  },
  {
    path: 'your-other-url',
    component: YourOtherComponent
  },
 {
    path: '',                    //para direcionar a home
    redirectTo: '/your-url',
    pathMatch: 'full'
  }
];
```

Nos componentes .html
```
<button routerLink="/your-url"></button>
```

No component.ts
```
import { RouterLink }
...
imports: [...
         RouterLink]
```

Para usar na lógica do componente
```
this.router.navigateByUrl('/your-page');
```

<br><br>


## Dependency Injection (Services)

```
import {Injectable} from '@angular/core';

@Injectable({
  providedIn: 'root',
})

export class YourService {
  //your logic...
}
```

To inject
```
import {Component, inject} from '@angular/core';
import {YourService} from './your-service.service';

@Component({
  selector: 'app-root',
  template: `
    <p>Your Listing: {{ display }}</p>
  `,
})
export class App {
  yourService = inject(YourService);

  display = this.yourService.getYourModel().join(' ⭐️ ');
}
```

Or use the constructor + ngOnInit
```
...
import { Component, OnInit } from '@angular/core';

export class YourComponent implements OnInit {...}

constructor(private yourService: YourService) {
...
}

ngOnInit() {
  this.yourProperty = this.yourService.getYourData();
}
```

<br><br>

## Pipes
Special operator in Angular template expressions that allows you to transform data. Pipes are functions that accept an input value and return a transformed value. 

Angular pipes use the vertical bar character (|).

There some built-in pipelines that you can import or your can create your own: https://angular.dev/guide/templates/pipes

You can use multiple pipes and pass parameters:

```
<p>The event will occur on {{ scheduledOn | date | uppercase }}.</p>

<p>The event will occur at {{ scheduledOn | date:'hh:mm':'UTC' }}.</p>
```

Always use parentheses in your expressions when operator precedence may be ambiguous. Because the pipe operator has lower precedence than other binary operators, including +, -, *, /, %, &&, ||, and ??. And has higher precedence than the conditional (ternary) operator.

```
{{ (isAdmin ? 'Access granted' : 'Access denied') | uppercase }}
```


<br><br>

## To optimize image loading

https://angular.dev/tutorials/learn-angular/11-optimizing-images

<br><br>


## HttpClient

Para simular um backend, você pode usar o JSON-Server (https://www.npmjs.com/package/json-server?activeTab=readme)

https://angular.dev/guide/http

Add the provider, in app.config.ts or NgModule

```
export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(),
  ]
};
```

Inject in your componenets or services
```
@Injectable({providedIn: 'root'})
export class ConfigService {
  private http = inject(HttpClient);
  // This service can now make HTTP requests via `this.http`.
}
```

Or through the constructor (tutorial)
```
import { HttpClient } from '@angular/common/http';
...
constructor(private http: HttpClient) {}
```

Fetch JSON data no seu SERVICE
O retorno é sempre um Observable, então precisa importar
```
import { Observable } from 'rxjs';

listarDados(): Observable<yourType> {
  return this.http.get<yourType>('/apiUrl');
}
```

INSCREVA o método para receber os dados
```
import { YourService } from '../../services/yourService.service';
...

ngOnInit() {
  this.yourService.listarDados().subscribe(yourData => {
    this.yourPorperty = yourData;
  });
}
```

