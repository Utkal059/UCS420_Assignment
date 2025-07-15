#include <iostream>
#include <vector>
#include <algorithm> // Required for the sort function
#include <iomanip>   // Required for formatting output (setw)

// A structure to represent a process
struct Process {
    int id;         // Process ID
    int burst_time; // Time required for execution
};

// A comparison function used by sort() to order processes by burst time
bool compareByBurstTime(const Process& a, const Process& b) {
    return a.burst_time < b.burst_time;
}

int main() {
    int n;

    std::cout << "Enter the number of processes: ";
    std::cin >> n;

    std::vector<Process> processes(n);

    // Get the burst time for each process from the user
    for (int i = 0; i < n; ++i) {
        processes[i].id = i + 1;
        std::cout << "Enter burst time for process " << (i + 1) << ": ";
        std::cin >> processes[i].burst_time;
    }

    // Sort the processes based on their burst time (the core of SJF)
    std::sort(processes.begin(), processes.end(), compareByBurstTime);

    std::vector<int> waiting_time(n);
    std::vector<int> turnaround_time(n);
    float total_waiting_time = 0;
    float total_turnaround_time = 0;

    // The first process has a waiting time of 0
    waiting_time[0] = 0;

    // Calculate waiting time for all other processes
    for (int i = 1; i < n; ++i) {
        waiting_time[i] = waiting_time[i - 1] + processes[i - 1].burst_time;
    }
    
    std::cout << "\n--- SJF Scheduling Results ---" << std::endl;
    std::cout << std::setw(10) << "Process" << std::setw(15) << "Burst Time"
              << std::setw(15) << "Waiting Time" << std::setw(20) << "Turnaround Time" << std::endl;

    // Calculate turnaround time and display results for each process
    for (int i = 0; i < n; ++i) {
        turnaround_time[i] = processes[i].burst_time + waiting_time[i];
        
        total_waiting_time += waiting_time[i];
        total_turnaround_time += turnaround_time[i];

        std::cout << std::setw(10) << processes[i].id
                  << std::setw(15) << processes[i].burst_time
                  << std::setw(15) << waiting_time[i]
                  << std::setw(20) << turnaround_time[i] << std::endl;
    }

    // Display the average waiting and turnaround times
    std::cout << std::fixed << std::setprecision(2); // Format output to 2 decimal places
    std::cout << "\nAverage Waiting Time: " << total_waiting_time / n << std::endl;
    std::cout << "Average Turnaround Time: " << total_turnaround_time / n << std::endl;

    return 0;
}
