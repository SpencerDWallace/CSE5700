/********************************************************************************
Please enter the input text below after executing the program, then
press the enter key, if necessary, until there is a blank line following
the input text. The output should then be produced.
********************************************************************************/
/*
These are a few words for my C programming
exercise. My name is Spencer Wallace
and I love Computer Science.
*/

#include <stdio.h>
#define MAX 150

int main() {
  int num_characters = 0;
  int num_words = 0;
  int num_lines = 0;
  char buffer[MAX];

  // reads the input until the first character of the current line is the
  // newline character
  while (fgets(buffer, sizeof(buffer), stdin) && buffer[0] != '\n') {
    for (int i = 0; i < strlen(buffer); i++) {
      if (buffer[i] == ' ')
        num_words++;
      if (buffer[i] == '\n') {
        num_words++;
        num_lines++;
      } else // any character besides newline
        num_characters++;
    }
  }

  printf("Number of characters: %d", num_characters);
  printf("\nNumber of words: %d", num_words);
  printf("\nNumber of lines: %d\n", num_lines);

  return 0;
}