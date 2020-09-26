#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
main()
{
  pid_t pid;
  int act;
  switch(pid=fork()) {
  case -1:
          perror("fork"); /* произошла ошибка */
          exit(1); /*выход из родительского процесса*/
  case 0:
          printf(" CHILD: Это процесс-потомок!\n");
          execvp(argv[0], &argv[0]);
          exit(1);
  default:
          printf("PARENT: Это процесс-родитель!\n");
          wait(&act); //ждёт завершения дочернего процесса
          printf("PARENT: Код возврата потомка:%d\n", WEXITSTATUS(act));  //возращение восьми битов, который верпнул завершившийся дочерний процесс
  }
}
