%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int yyparse();
int yylex();
void yyerror(const char *s);

%}

%union {
    char *str;
}

%token <str> BINARY

%%

input:
    input line
    |
    ;

line:
    BINARY {
        int decimal = 0;
        char *bin = $1;

        for (int i = 0; bin[i] != '\0'; i++) {
            decimal = decimal * 2 + (bin[i] - '0');
        }

        printf("Binary: %s -> Decimal: %d\n", bin, decimal);
        free(bin);
    }
    ;

%%

void yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
}

int main() {
    printf("Enter binary numbers (Ctrl+D to end):\n");
    yyparse();
    return 0;
}
