# Respuestas

## 1_e

La principal diferencia entre ambos arrays es que el array estatico se almacena en el stack y el dinamico en el heap (el de malloc).

## 2_a

Guarda la sumatoria de todos los productos del array en el ptr_sum y la productoria en ptr_prod

## 2_b

Copia una string en otra posicion de memoria y devuelve el puntero por parametro:

```C
 if(*destination != NULL) free(*destination); 
```

si el puntero dest es distinto de NULL libera la memoria.

```C
if(source != NULL){
    //strlen devuelve el largo de un string sin contar el caracter de cierre
    *destination = malloc(strlen(source) + 1);
    //strcpy copia el contenido de un string en otro, incluyendo el caracter de cierre
    strcpy(*destination, source);
    }
```

Si la string no esta inicializada, entonces reserva memoria para el dest y copia la string.

```C
else{*destination = NULL;}
```

devuelve NULL caso contrario.

## 2_e

Si se usa un solo ptr tira seg fault.

## 2_f

Si solo se hace free al dinamic_item se pierde el acceso al ptr del char interno y ya no se puede liberarlo, quedando como consecuencia memoria colgada.

## 2_g

Liberar primero los elementos mas internos y luego los mas externos hasta finalmente liberar la struct o array al final.

```C
struct t{
    struct s{
        int* p;
    }
    int *d;
}
struct t t_elem*{...}
free(t.s.p);
...
...
free(t)
```
