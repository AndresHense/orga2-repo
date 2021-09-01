# Answers to questions

## Checkpoint 1

a) 

* Se busca el atributo mas grande dentro del struct, y a partir de este se generan bloques de su tamaño para que la struct quede alineada al tamaño de este atributo, luego cada atributo particular es posicionado en uno de estos bloques de size **max_attribute**.


```C
struct t{uint32_t a,char b, char c, uint64_t d};
```

En este caso habrian dos bloques de 64bits:

a(32bits) +b(8bits) + c(8bits) + padding(16bits)=  bloque1
d(64bits) = bloque2

* Usemos la estructura t recien definida, si quisieramos acceder al atributo **c**, hariamos:

```C
    int p=*(&t+sizeof(typeof(t.a))+sizeof(typeof(t.b))

    struct t{uint32_t a,uint64_t e,char b, char c, uint64_t d};

    int p2=*(&t+sizeof(typeof(t.e)*2+sizeof(typeof(t.b)))) //asigno el valor de t.c
```

De estos dos ejemplos se desprende que para calcular el desplazamiento se debe saber a priori la distribucion de los atributos en los distintos bloques creados a partir del tamaño del atibuto mas largo del struct.

b) Para calcular el tamaño de una struct empaquetada simplemente se suman los tamaños de los tipos de cada atributo.

Mientras que para una struct no empaquetada se suman la cantidad de bloques creados por el tamaño del atributo mas largo.

c) El contrato prestablecido para realizar y pasar información entre funciones de usuarios.

#### Convencion C (64bits)

* No volatiles: RBX, R12, R13, R14 y R15
* Valor de retorno: RAX enteros/punteros,XMM0 flotantes
* Entero,puntero: RDI, RSI, RDX, RCX, R8, R9(izq. a der.)
* Flotantes: XMM0, XMM1,...(izq. a der.)
* ¿No hay registros? PUSH a la pila(der. a izq.)
* Inv. de pila: Todo PUSH/SUB debe tener su POP/ADD
* Llamada func. C: pila alineada a 16 bits (SUB RSP, X)

#### Convencion C (32 bits)

* No volatiles: EBX, ESI y EDI
* Valor de retorno: EAX
* Par ́ametros: PUSH a la pila(der. a izq.)
* Inv. de pila: Todo PUSH/SUB debe tener su POP/ADD

d) En C la responsabilidad sobre el contrato recae en el compilador, el programador solo debe especificar el tipo de variable e.g(int, char).

En ASM sin embargo, el programador es responsable de asegurar el contrato, para esto se debe alinear el stack, pasar los parametros por los registros correspondientes, etc.

e) El area del stack en donde estan los parametros que recibe la funcion y los que genera para poder volver retornar al lugar donde fue llamado.

* **Prologo:** Es la sección de codigo previa a la llamada de una función, este es compuesto por la reservación de espacio, la alineación de la pila y el pusheo de los registros no volatiles.

* **Epilogo:** Es la seccion de codigo posterior a la llamada de una funcion, en este caso se restauran los registros no volatiles, y se devuelve la pila al estado inicial.

```armasm
    push rbp    
    mov rsp,rbp 
    push r8     ;prologue
    add rsp,8   ;prologue
    mov rdi, 2  ;prologue

    call foo

    sub rsp,8   ;epilogue
    pop r8      ;epilogue
    
    pop rbp
    ret
```

f) En C, las variables temporales son almacenadas en el stack, mas puntualmente en las posiciones subsiguientes a donde se pushea el rbp al armarse el stackframe.

```C
int foo1(int a){
    int temp=0;
    return foo2(temp+a);
}
```

puedo acceder a temp desde foo2?

```armasm
foo2:
    push rbp    ;este rbp es el de foo1?
    mov rsp,rbp 
    ..
    
    mov r8,0x3
    mov r9,[rbp+0x2] ; [rbp+0x2] = temp
    ....
    ...
    ..

    pop rbp
    ret
```

g) Para 64bits, la pila debe estar alineada a 16bits, y para 32bits, la pila debe estar alineada a 8bits (que siempre lo esta).

Despues de entrar a una funcion y ejecutar la primera instruccion, la pila queda alineada a 8bits; ie esta deslineada.

h) Si tuvieramos un codigo en asm que pase un num de 64bits a foo3, luego de que se actualize la funcion, mi programa seguiria pasando los 64bits y foo3 solo leeria los primeros 32bits, por lo cual puede llegar a interpretar mal el numero pasado.

```C

int foo3(uint64_t a){
    return a+1;
}
...
...
...
//se modifica
int foo3(uint32_t a){
    return a+1;
}
```

## Checkpoint 2

### alternate_sum_4

```C
uint32_t alternate_sum_4(uint32_t x1, uint32_t x2, uint32_t x3, uint32_t x4);

```

Esto devuelve **x1-x2+x3-x4**

### sum_z

```C
uint32_t sum_z(simple_item *arr, uint32_t arr_length);
```

devuelve sumatoria(i...|arr|, arr[i].z)

### packed_sum_z

### product_2

### product_2_f
