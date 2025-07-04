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

//na classe


```

<br><br>

## Diretivas

### @if () {} @else {}

### @for (item of yourItems; track item.id) {}


