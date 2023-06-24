A Kristen le encanta jugar y comparar números. Ella piensa que si toma dos números positivos diferentes, aquel cuyos dígitos suman un número mayor es mejor que el otro. Si la suma de los dígitos es igual para ambos números, entonces piensa que el número más pequeño es mejor . Por ejemplo, Kristen piensa quees mejor quey esoes mejor que.

Dado un número entero, puedes encontrar el divisor deque Kristin considerará la mejor?
Restricciones: n debe ser mayor a 0 y menor a 100,000.

Entrada: Ingresar un dato n (entero)
salida:Imprime un n entero que es el mejor divisor

variables utilizadas:
En la función sumdiv:

sum - entero utilizado para almacenar la suma de los dígitos.
En la función bestDivisor:

md - entero utilizado para almacenar el divisor más grande.
ms - entero utilizado para almacenar la suma más grande de los dígitos de los divisores.
dv - entero utilizado como contador en el bucle for para iterar sobre los posibles divisores.
En la función main:

n - entero utilizado para almacenar el número de entrada.
En la función readline:

alloc_length - tamaño inicial de memoria asignada para almacenar la línea de entrada.
data_length - tamaño actual de la línea de entrada.
data - puntero a caracteres utilizado para almacenar la línea de entrada.
En la función ltrim y rtrim:

str - puntero a caracteres que representa una cadena.
En la función parse_int:

endptr - puntero a caracteres utilizado para indicar el final de la conversión.
value - entero utilizado para almacenar el valor convertido de la cadena.
Estas variables se utilizan para almacenar y manipular datos en el programa

Instrucciones para compilar:
1.Ejecutar el programa a traves del cmd con gcc (con un compilar Win32 previamente instalado)

Siguiendo las reglas de Kristen, y las restricciones tenemos que la solución es 
Creamos variables para guardar el mejor divisor y sus sumas de dígitos 
Para cada divisor, se verifica si el número n es divisible por ese divisor utilizando el operador de módulo (%). Si el residuo es igual a cero, significa que es un divisor válido.
Se calcula la suma de dígitos del  divisor  y al final el valor almacenado será el mejor divisor


#include <assert.h>
#include <ctype.h>
#include <limits.h>
#include <math.h>
#include <stdbool.h>
#include <stddef.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* readline(); // permiten que el compilador conozca la firma de estas 
//funciones antes de que sean definidas más adelante en el código.
char* ltrim(char*);
char* rtrim(char*);

int parse_int(char*);

int sumdiv(int n) { //calcula la suma de los dígitos de un número entero
    int sum = 0;
    while (n > 0) {
        sum += n % 10;// OBTIENE CADA DIGITO Y LO AGREGA A LA SUMA
        n /= 10;
    }
    return sum;
}

int bestDivisor(int N) { //La función itera desde 2 hasta N y verifica si cada número es un divisor de N. 
//Si lo es, calcula la suma de los dígitos utilizando la función sumdiv y compara esta suma con la suma actual más grande (ms). Si la suma es mayor que la suma más grande actual o si es igual pero el divisor es más pequeño, se actualiza la suma más grande y el divisor.
    int md = 1;
    int ms = 1;
    int dv;
    for (dv = 2; dv <= N; dv++) {
        if (N % dv == 0) {
            int sum_ac = sumdiv(dv);
            if (sum_ac > ms || (sum_ac == ms && dv < md)) {
                md = dv;
                ms = sum_ac;
            }
        }
    }
    return md;
}

int main() {
    int n = parse_int(ltrim(rtrim(readline())));

    if (n > 0 && n < 100000) { //SE VERIFICA QUE EL NUMERO CUMPLA LA RESTRICCIÓN
        int divm = bestDivisor(n);
        printf("El divisor de %d que se considera mejor es: %d\n", n, divm);
    } else {
        printf("El número ingresado no cumple con las restricciones dadas :(\n");
    }

    return 0;
}
//son funciones utilizadas para leer una línea de entrada, eliminar los espacios en blanco iniciales y finales
//convertir una cadena en un número entero
char* readline() {
    size_t alloc_length = 1024;
    size_t data_length = 0;

    char* data = malloc(alloc_length);

    while (true) {
        char* cursor = data + data_length;
        char* line = fgets(cursor, alloc_length - data_length, stdin);

        if (!line) {
            break;
        }

        data_length += strlen(cursor);

        if (data_length < alloc_length - 1 || data[data_length - 1] == '\n') {
            break;
        }

        alloc_length <<= 1;

        data = realloc(data, alloc_length);

        if (!data) {
            data = '\0';
            break;
        }
    }

    if (data[data_length - 1] == '\n') {
        data[data_length - 1] = '\0';
        data = realloc(data, data_length);

        if (!data) {
            data = '\0';
        }
    } else {
        data = realloc(data, data_length + 1);

        if (!data) {
            data = '\0';
        } else {
            data[data_length] = '\0';
        }
    }

    return data;
}

char* ltrim(char* str) {
    if (!str) {
        return '\0';
    }

    if (!*str) {
        return str;
    }

    while (*str != '\0' && isspace(*str)) {
        str++;
    }

    return str;
}

char* rtrim(char* str) {
    if (!str) {
        return '\0';
    }

    if (!*str) {
        return str;
    }

    char* end = str + strlen(str) - 1;

    while (end >= str && isspace(*end)) {
        end--;
    }

    *(end + 1) = '\0';

    return str;
}

int parse_int(char* str) {
    char* endptr;
    int value = strtol(str, &endptr, 10);

    if (endptr == str || *endptr != '\0') {
        exit(EXIT_FAILURE);
    }

    return value;
}


