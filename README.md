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

<ng-content />

<br><br>

### @for (item of yourItems; track item.id) {}

