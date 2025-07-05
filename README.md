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

```this.yourForm.value```

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

## Router

Faça a importação
```
import { RouterOutlet } from '@angular/router';
...
imports: [...
         RouterOutlet],
```

Adicione no app.component.html
```
<router-outlet />
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


<br><br>

## To optimize image loading

https://angular.dev/tutorials/learn-angular/11-optimizing-images
