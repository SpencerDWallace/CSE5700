/*Instructor: Dr. Ngobi*/
/*Course:: CSE 5700 - Compilers*/
/*Student: Spencer Wallace*/
/*Assignment: Lab 2*/

/***** Source Code *****/
#include <ctype.h>
#include <stdio.h>
#include <string.h>

void keyword(char str[10]) {
  if (strcmp("for", str) == 0 || strcmp("while", str) == 0 ||
      strcmp("do", str) == 0 || strcmp("int", str) == 0 ||
      strcmp("float", str) == 0 || strcmp("char", str) == 0 ||
      strcmp("double", str) == 0 || strcmp("static", str) == 0 ||
      strcmp("switch", str) == 0 || strcmp("case", str) == 0)
    printf("\n%s is a keyword", str);
  else
    printf("\n%s is an identifier", str);
}

int isRelationalOperator(char c) {
  if (c == '<' || c == '>')
    return 1;
  else
    return 0;
}

int main() {
  FILE *f1, *f2, *f3, *f4;
  char c, prevC, str[10], st1[10];
  int num[100], lineno = 0, tokenvalue = 0, i = 0, j = 0, k = 0;
  printf(
      "\n Enter the input in the program. Input at least 3 lines by pressing "
      "Enter at the end of each line. When done, press CTRL+D: "); /*gets(st1);*/
  f1 = fopen("input", "w");
  while ((c = getchar()) != EOF)
    putc(c, f1);
  fclose(f1);
  f1 = fopen("input", "r");
  f2 = fopen("identifier", "w");
  f3 = fopen("specialchar", "w");
  f4 = fopen("relationaloper", "w");

  prevC = '@'; /*used to check if current c is the first char of the file f1 and to track the previous char read*/
  while ((c = getc(f1)) != EOF) {
    if (isdigit(c)) {
      tokenvalue = c - '0';
      c = getc(f1);
      while (isdigit(c)) {
        tokenvalue *= 10 + c - '0';
        c = getc(f1);
      }
      num[i++] = tokenvalue;
      ungetc(c, f1);
    } else if (isalpha(c)) {
      putc(c, f2);
      c = getc(f1);
      while (isdigit(c) || isalpha(c) || c == '_' || c == '$') {
        putc(c, f2);
        c = getc(f1);
      }
      putc(' ', f2);
      ungetc(c, f1);
    } else if (c == ' ' || c == '\t')
      printf(" ");
    else if (c == '\n')
      lineno++;
    else {
      if (prevC != '@' && c == '=' && isRelationalOperator(prevC)) {
        putc(c, f4);
      } else if (isRelationalOperator(c)) {
        putc(c, f4);
      } else {
        putc(c, f3);
      }
    }
    prevC = c;
  }
  fclose(f1);
  fclose(f2);
  fclose(f3);
  fclose(f4);
  printf("\n The no's in the program are :");
  for (j = 0; j < i; j++)
    printf("%d", num[j]);
  printf("\n");
  f2 = fopen("identifier", "r");
  k = 0;
  printf("The keywords and identifiers are:");
  while ((c = getc(f2)) != EOF) {
    if (c != ' ')
      str[k++] = c;
    else {
      str[k] = '\0';
      keyword(str);
      k = 0;
    }
  }
  fclose(f2);
  f3 = fopen("specialchar", "r");
  printf("\nSpecial characters are : ");
  while ((c = getc(f3)) != EOF)
    printf("%c", c);
  printf("\n");
  fclose(f3);

  f4 = fopen("relationaloper", "r");
  printf("Relation operators are : ");
  while ((c = getc(f4)) != EOF)
    printf("%c", c);
  printf("\n");
  fclose(f4);

  printf("Total no. of lines are:%d\n", lineno);
}


/***** Sample Output *****/
/*
Enter the input in the program. Input at least 3 lines 
by pressing Enter at the end of each line. 
When done, press CTRL+D: 
while that is true then 4 is <= 5
since 7 >= 3 then 9 > 6
and then 1 is >= 1 but < 2 
finally 3+4 is > 5-2 since 3+4 = 7
                               
The no's in the program are :4573961123452347
The keywords and identifiers are:
while is a keyword
that is an identifier
is is an identifier
true is an identifier
then is an identifier
is is an identifier
since is an identifier
then is an identifier
and is an identifier
then is an identifier
is is an identifier
but is an identifier
finally is an identifier
is is an identifier
since is an identifier
Special characters are : +-+=
Relation operators are : <=>=>>=<>
Total no. of lines are:4
*/