#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd, ret;
    char buffer[20];

    // Create a new file
    fd = creat("example.txt", 0644);
    if (fd == -1) {
        perror("creat");
        exit(EXIT_FAILURE);
    }

    // Write to the file
    ret = write(fd, "Hello, World!", 13);
    if (ret == -1) {
        perror("write");
        exit(EXIT_FAILURE);
    }

    // Close the file
    ret = close(fd);
    if (ret == -1) {
        perror("close");
        exit(EXIT_FAILURE);
    }

    // Open the file again
    fd = open("example.txt", O_RDWR);
    if (fd == -1) {
        perror("open");
        exit(EXIT_FAILURE);
    }

    // Move the file cursor to the beginning of the file
    ret = lseek(fd, 0, SEEK_SET);
    if (ret == -1) {
        perror("lseek");
        exit(EXIT_FAILURE);
    }

    // Read from the file
    ret = read(fd, buffer, 13);
    if (ret == -1) {
        perror("read");
        exit(EXIT_FAILURE);
    }
    buffer[ret] = '\0'; // Null-terminate the string

    printf("Read from file: %s\n", buffer);

    // Close the file
    ret = close(fd);
    if (ret == -1) {
        perror("close");
        exit(EXIT_FAILURE);
    }
    return 0;
}
