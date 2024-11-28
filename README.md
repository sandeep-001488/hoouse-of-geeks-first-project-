#include <stdio.h>
#include <stdlib.h>
#include <time.h>

typedef struct {
    int id;
    int execution_time;
    int priority;
} Process;

void generate_processes(Process processes[], int n) {
    for (int i = 0; i < n; i++) {
        processes[i].id = i + 1;
        processes[i].execution_time = rand() % 20 + 1; // Random execution time between 1 and 20
        processes[i].priority = rand() % 10 + 1;      // Random priority between 1 and 10
    }
}


void sort_by_id(Process processes[], int n) {
    // No sorting needed as processes are already in order of their ID
}

void sort_by_execution_time(Process processes[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (processes[i].execution_time > processes[j].execution_time) {
                Process temp = processes[i];
                processes[i] = processes[j];
                processes[j] = temp;
            }
        }
    }
}

void sort_by_priority(Process processes[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (processes[i].priority > processes[j].priority) {
                Process temp = processes[i];
                processes[i] = processes[j];
                processes[j] = temp;
            }
        }
    }
}

void fcfs_scheduling(Process processes[], int n) {
    int waiting_time = 0, turnaround_time = 0;
    int total_wt = 0, total_tat = 0;
    int completion_time = 0;

    printf("\nFCFS Scheduling:\n");
    printf("Process ID\tExecution Time\tWaiting Time\tTurnaround Time\n");

    for (int i = 0; i < n; i++) {
        waiting_time = completion_time;
        turnaround_time = waiting_time + processes[i].execution_time;

        total_wt += waiting_time;
        total_tat += turnaround_time;

        printf("%d\t\t%d\t\t%d\t\t%d\n", processes[i].id, processes[i].execution_time, waiting_time, turnaround_time);

        completion_time += processes[i].execution_time;
    }

    printf("Average Waiting Time: %.2f\n", (float)total_wt / n);
    printf("Average Turnaround Time: %.2f\n", (float)total_tat / n);
}

void sjf_scheduling(Process processes[], int n) {
    int waiting_time = 0, turnaround_time = 0;
    int total_wt = 0, total_tat = 0;
    int completion_time = 0;

    printf("\nSJF Scheduling:\n");
    printf("Process ID\tExecution Time\tWaiting Time\tTurnaround Time\n");

    for (int i = 0; i < n; i++) {
        waiting_time = completion_time;
        turnaround_time = waiting_time + processes[i].execution_time;

        total_wt += waiting_time;
        total_tat += turnaround_time;

        printf("%d\t\t%d\t\t%d\t\t%d\n", processes[i].id, processes[i].execution_time, waiting_time, turnaround_time);

        completion_time += processes[i].execution_time;
    }

    printf("Average Waiting Time: %.2f\n", (float)total_wt / n);
    printf("Average Turnaround Time: %.2f\n", (float)total_tat / n);
}

void priority_scheduling(Process processes[], int n) {
    int waiting_time = 0, turnaround_time = 0;
    int total_wt = 0, total_tat = 0;
    int completion_time = 0;

    printf("\nPriority Scheduling:\n");
    printf("Process ID\tExecution Time\tPriority\tWaiting Time\tTurnaround Time\n");

    for (int i = 0; i < n; i++) {
        waiting_time = completion_time;
        turnaround_time = waiting_time + processes[i].execution_time;

        total_wt += waiting_time;
        total_tat += turnaround_time;

        printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\n", processes[i].id, processes[i].execution_time, processes[i].priority, waiting_time, turnaround_time);

        completion_time += processes[i].execution_time;
    }

    printf("Average Waiting Time: %.2f\n", (float)total_wt / n);
    printf("Average Turnaround Time: %.2f\n", (float)total_tat / n);
}

int main() {
    srand(time(NULL)); // Seed for random number generation

    int num_processes = 7;
    Process processes[num_processes];

    generate_processes(processes, num_processes);

    printf("Generated Processes (ID, Execution Time, Priority):\n");
    for (int i = 0; i < num_processes; i++) {
        printf("Process %d: Execution Time = %d, Priority = %d\n", processes[i].id, processes[i].execution_time, processes[i].priority);
    }

    sort_by_id(processes, num_processes);
    fcfs_scheduling(processes, num_processes);

    // SJF Scheduling
    sort_by_execution_time(processes, num_processes);
    sjf_scheduling(processes, num_processes);

    // Priority Scheduling
    sort_by_priority(processes, num_processes);
    priority_scheduling(processes, num_processes);

    return 0;
}


[28-11-2024 12:31] Sandeep Barnwal: gcc -E program.c -o program.i
[28-11-2024 12:31] Sandeep Barnwal: gcc -S program.c -o program.s
[28-11-2024 12:32] Sandeep Barnwal: gcc -c program.c -o program.o
[28-11-2024 12:32] Sandeep Barnwal: all together

gcc program.c -o program


//fcfs disk

#include <stdio.h>
#include <stdlib.h>

void fcfs(int requests[], int n, int head) {
    int seek_time = 0;
    printf("FCFS Disk Scheduling:\n");
    printf("Sequence: %d -> ", head);

    for (int i = 0; i < n; i++) {
        seek_time += abs(requests[i] - head);
        head = requests[i];
        printf("%d -> ", head);
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / n);
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;

    fcfs(requests, n, head);
    return 0;
}

///shortest-seek timee first

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

void sstf(int requests[], int n, int head) {
    int seek_time = 0, completed = 0;
    bool visited[n];
    for (int i = 0; i < n; i++) visited[i] = false;

    printf("SSTF Disk Scheduling:\n");
    printf("Sequence: %d -> ", head);

    while (completed < n) {
        int min_dist = 1e9, next = -1;

        for (int i = 0; i < n; i++) {
            if (!visited[i] && abs(requests[i] - head) < min_dist) {
                min_dist = abs(requests[i] - head);
                next = i;
            }
        }

        visited[next] = true;
        seek_time += min_dist;
        head = requests[next];
        printf("%d -> ", head);
        completed++;
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / n);
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;

    sstf(requests, n, head);
    return 0;
}


//scan

#include <stdio.h>
#include <stdlib.h>

int compare(const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}

void scan(int requests[], int n, int head, int disk_size) {
    int seek_time = 0;
    int sorted[n + 1];
    int pos = 0;

    for (int i = 0; i < n; i++) sorted[i] = requests[i];
    sorted[n] = head; // Add the head to the requests array
    n++;

    qsort(sorted, n, sizeof(int), compare);

    // Find the position of the head in the sorted array
    for (int i = 0; i < n; i++) {
        if (sorted[i] == head) {
            pos = i;
            break;
        }
    }

    printf("SCAN Disk Scheduling:\n");
    printf("Sequence: ");

    // Move in the direction towards the end
    for (int i = pos; i < n; i++) {
        printf("%d -> ", sorted[i]);
        if (i > pos) seek_time += abs(sorted[i] - sorted[i - 1]);
    }
    
    // Move in the reverse direction
    for (int i = pos - 1; i >= 0; i--) {
        printf("%d -> ", sorted[i]);
        seek_time += abs(sorted[i] - sorted[i + 1]);
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / (n - 1));
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;
    int disk_size = 200;

    scan(requests, n, head, disk_size);
    return 0;
}


// circular scan

#include <stdio.h>
#include <stdlib.h>

void cscan(int requests[], int n, int head, int disk_size) {
    int seek_time = 0;
    int sorted[n + 2]; // Add head and 0 or max disk size
    int pos = 0;

    for (int i = 0; i < n; i++) sorted[i] = requests[i];
    sorted[n] = head;
    sorted[n + 1] = disk_size - 1; // End of the disk
    n += 2;

    qsort(sorted, n, sizeof(int), compare);

    // Find the position of the head
    for (int i = 0; i < n; i++) {
        if (sorted[i] == head) {
            pos = i;
            break;
        }
    }

    printf("C-SCAN Disk Scheduling:\n");
    printf("Sequence: ");

    // Move towards the end of the disk
    for (int i = pos; i < n; i++) {
        printf("%d -> ", sorted[i]);
        if (i > pos) seek_time += abs(sorted[i] - sorted[i - 1]);
    }

    // Jump to the beginning and move towards the head
    seek_time += abs(sorted[n - 1] - 0); // Jump to start
    printf("0 -> ");
    for (int i = 0; i < pos; i++) {
        printf("%d -> ", sorted[i]);
        if (i > 0) seek_time += abs(sorted[i] - sorted[i - 1]);
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / (n - 2));
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;
    int disk_size = 200;

    cscan(requests, n, head, disk_size);
    return 0;
}




//look

void look(int requests[], int n, int head) {
    int seek_time = 0;
    int sorted[n + 1];
    int pos = 0;

    for (int i = 0; i < n; i++) sorted[i] = requests[i];
    sorted[n] = head; // Add head to requests
    n++;

    qsort(sorted, n, sizeof(int), compare);

    // Find the position of the head
    for (int i = 0; i < n; i++) {
        if (sorted[i] == head) {
            pos = i;
            break;
        }
    }

    printf("LOOK Disk Scheduling:\n");
    printf("Sequence: ");

    // Move in the forward direction
    for (int i = pos; i < n; i++) {
        printf("%d -> ", sorted[i]);
        if (i > pos) seek_time += abs(sorted[i] - sorted[i - 1]);
    }

    // Move in the reverse direction
    for (int i = pos - 1; i >= 0; i--) {
        printf("%d -> ", sorted[i]);
        seek_time += abs(sorted[i] - sorted[i + 1]);
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / (n - 1));
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;

    look(requests, n, head);
    return 0;
}


// circular look

void clook(int requests[], int n, int head) {
    int seek_time = 0;
    int sorted[n + 1];
    int pos = 0;

    for (int i = 0; i < n; i++) sorted[i] = requests[i];
    sorted[n] = head;
    n++;

    qsort(sorted, n, sizeof(int), compare);

    // Find the position of the head
    for (int i = 0; i < n; i++) {
        if (sorted[i] == head) {
            pos = i;
            break;
        }
    }

    printf("C-LOOK Disk Scheduling:\n");
    printf("Sequence: ");

    // Move towards the end
    for (int i = pos; i < n; i++) {
        printf("%d -> ", sorted[i]);
        if (i > pos) seek_time += abs(sorted[i] - sorted[i - 1]);
    }

    // Jump to the start and move towards the head
    seek_time += abs(sorted[n - 1] - sorted[0]);
    for (int i = 0; i < pos; i++) {
        printf("%d -> ", sorted[i]);
        if (i > 0) seek_time += abs(sorted[i] - sorted[i - 1]);
    }

    printf("\nTotal Seek Time: %d\n", seek_time);
    printf("Average Seek Time: %.2f\n", (float)seek_time / (n - 1));
}

int main() {
    int requests[] = {98, 183, 37, 122, 14, 124, 65, 67};
    int n = sizeof(requests) / sizeof(requests[0]);
    int head = 53;

    clook(requests, n, head);
    return 0;
}

