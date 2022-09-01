
ReviewTableCell.vue (условно)

Как предлагаю я:
```
<template>
    <div class="cell">
        <div class="message">Message</div>
        <div class="describe">Describe</div>
    </div>
</template>

<style scoped>
 .cell {
    padding: 30px;
    color: black;
 }
 
 .message {
    font-weight: 500;
    font-size: 14px;
    
    margin-bottom: 30px;
 }
 
 .describe {
    font-size: 12px;
 }
</style>
```

Как это выглядело бы по BEM:

ReviewTableCell.vue (условно)
```
<template>
    <div class="review-table-cell">
        // согласно BEM мы не можем здесь использовать класс message, 
        // если хотим задать стили, которые видны только в этом компоненте 
        <div class="review-table-cell-message review-table-cell__message">Message</div>
        // по такой же логике не можем использовать класс describe 
        <div class="review-table-cell-describe">Describe</div>
    </div>
</template>

<style> // в таком случае и scoped не нужен
 .review-table-cell {
    padding: 30px;
    color: black;
 }
 
 .review-table-cell-message { 
    font-weight: 500;
    font-size: 14px;    
 }
 
 .review-table-cell__message { // внешние стили выносятся в элемент (по BEM так)
    margin-bottom: 30px;
 }
 
 .review-table-cell-describe {
    font-size: 12px;
 }
</style>
```

В scoped стилях нет смысла в элементах BEM, 
потому что то, что прописано в scoped не может переиспользоваться в других местах 
и внешние стили можно задавать вместе с общими стилями.

```
.message {
    font-weight: 500;
    font-size: 14px;
    
    margin-bottom: 30px; // уже не BEM, но для scoped стилей это логично
}
```

Отмечу, что не стоит задавать внешние стили для корневого элемента компонента:

```
<template>
    <div class="cell">

    </div>
</template>

<style>
    .cell {
        color: red;
        
        margin-bottom: 10px; // не надо так
    }
    
    .cell {
        position: absolute;
        inset: 10px 20px 30px 40px; // так тоже не надо
    }
</style>
```

Почему? В BEM нельзя давать блокам внешние стили, потому что они являются переиспользуемой единицей и могут использоваться во многих местах.
В случае со Vue переиспользуемой единицей является компонент и внешние стили для корневого элмемента будут мешать нормально использовать компонент в других местах.


Окей, все выглядит круто, на простом шаблоне, рассмотрим пример по-сложнее

Review.vue
Так это бы выглядело бы с BEM (элементы опустил, из-за причин выше)
```
<template>
    <section class="review">
        <header class="review-header"></header>
    
        <table class="review-users-table">
             <thead class="review-users-table-thead"></thead>
             <tbody class="review-users-table-tbody"></tbody>
        </table>
        
        <div class="review-pagination"></div>
    </section> 
</template>
```

Так это бы выглядело бы без BEM:

Review.vue
```
<template>
// окей, можно review назвать, но и назвать wrapper или root или section я не считаю ошибкой, 
// на уровне стилей даже понятнее становится, что это корневой элемент
    <section class="review"> 
        // просто header, так как находится в компоненте Review и не нужно уточнять это
        <header class="header"></header> 
    
        // просто users-table, так как находится в компоненте Review и не нужно уточнять это
        <table class="users-table"> 
            // так как это не вынесено в компонент (хорошо бы вынести), мы не можем здесь опустить контекст users-table
             <thead class="users-table-thead"></thead>  
             <tbody class="users-table-tbody"></tbody>
        </table>
        
        <div class="pagination"></div> // просто pagination, потому что внутри компонента Review
    </section> 
</template>
```

Что поменяется:

1) Уходят BEM элементы (оказывается сейчас они даже не по логике BEM используются). То есть в проекте запись элемента 
__ используется много где не для задания внешних стилей, а по ощущениям, чтобы показать связность. 
По-моему, связность не через "__" показывается, а через контекст в классе, условно .table .table-thead.
2) Не нужно будет писать бессмысленные длинные названия классов "по BEM".


Некоторые сложности, которые могут быть:

Классы table, alert, badge, pagination и тд прокидываются в компонент из глобальных стилей бутстрапа.

Какой я вижу здесь выход из ситуации:

давать префикс, например app-table, app-pagination и тд.

В гитлабе есть свой префикс gl. Можем использовать valt, но как я понял, может меняться название проекта.