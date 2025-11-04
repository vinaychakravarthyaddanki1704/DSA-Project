#include <stdio.h>
#include <string.h>

#define MAX 100
#define INF 9999
#define PEAK1_START 900
#define PEAK1_END   1030
#define PEAK2_START 1630
#define PEAK2_END   1800

// Station arrays
const char* redLine[] = {
    "Miyapur","JNTU College","KPHB Colony","Kukatpally","Balanagar","Moosapet",
    "Bharat Nagar","Erragadda","ESI Hospital","S.R. Nagar","Ameerpet","Punjagutta",
    "Irrum Manzil","Khairatabad","Lakdi-ka-pul","Assembly","Nampally",
    "Gandhi Bhavan","Osmania Medical College","MGBS","Malakpet",
    "New Market","Chaderghat","Dilsukhnagar","Moosarambagh","LB Nagar"
};

const char* blueLine[] = {
    "Nagole","Uppal","Stadium","NGRI","Habsiguda","Tarnaka","Mettuguda",
    "Secunderabad East","Paradise","Prakash Nagar","Rasoolpura","Begumpet",
    "Ameerpet","Madhura Nagar","Yousufguda","Jubilee Hills Road No.5",
    "Jubilee Hills Check Post","Peddamma Temple","Madhapur","Durgam Cheruvu",
    "Hitec City","Raidurg"
};

const char* greenLine[] = {
    "JBS Parade Ground","Secunderabad West","Gandhi Hospital","Musheerabad",
    "RTC X Roads","Chikkadpally","Narayanguda","Sultan Bazar","MGBS"
};

int redCount = 26, blueCount = 22, greenCount = 9;

char allStations[MAX][50];
int n = 0; // total stations
int graph[MAX][MAX];

// Custom lowercase function without ctype.h
void toLower(char* str) {
    for (int i = 0; str[i]; i++) {
        if (str[i] >= 'A' && str[i] <= 'Z') {
            str[i] += 32;  // Convert ASCII uppercase to lowercase
        }
    }
}

// Linear search to find station index
int findStationIndex(const char* station) {
    char inputLower[50];
    strcpy(inputLower, station);
    toLower(inputLower);
    for (int i = 0; i < n; i++) {
        char temp[50];
        strcpy(temp, allStations[i]);
        toLower(temp);
        if (strcmp(temp, inputLower) == 0)
            return i;
    }
    return -1;
}

// Display stations line-wise
void displayStations(const char* lineName, const char* stations[], int count) {
    printf("\n--- %s ---\n", lineName);
    for (int i = 0; i < count; i++)
        printf("%d. %s\n", i + 1, stations[i]);
}

// Min distance for Dijkstra
int minDistance(int dist[], int visited[]) {
    int min = INF, min_index = -1;
    for (int i = 0; i < n; i++) {
        if (!visited[i] && dist[i] < min) {
            min = dist[i];
            min_index = i;
        }
    }
    return min_index;
}

// Print path recursively
void printPath(int parent[], int j) {
    if (parent[j] == -1) {
        printf("%s", allStations[j]);
        return;
    }
    printPath(parent, parent[j]);
    printf(" -> %s", allStations[j]);
}

// Dijkstra algorithm
void dijkstra(int src, int dest, int travelTime) {
    int dist[MAX], visited[MAX], parent[MAX];
    for (int i = 0; i < n; i++) {
        dist[i] = INF;
        visited[i] = 0;
        parent[i] = -1;
    }
    dist[src] = 0;

    for (int count = 0; count < n; count++) {
        int u = minDistance(dist, visited);
        if (u == -1) break;
        visited[u] = 1;

        for (int v = 0; v < n; v++) {
            if (!visited[v] && graph[u][v] != 0 && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }

    printf("\nShortest Path: ");
    printPath(parent, dest);
    printf("\nTotal Distance: %d km\n", dist[dest]);

    int fare = 20 + dist[dest]*5;
    if ((travelTime >= PEAK1_START && travelTime <= PEAK1_END) ||
        (travelTime >= PEAK2_START && travelTime <= PEAK2_END)) {
        fare += 10;
        printf("Peak hour surcharge applied.\n");
    }

    printf("Estimated Fare: Rs. %d\n", fare);
}

// Plan trip
void planTrip() {
    char srcName[50], destName[50];
    int srcIndex, destIndex, travelTime;

    printf("Enter source station: ");
    scanf(" %[^\n]", srcName);
    printf("Enter destination station: ");
    scanf(" %[^\n]", destName);
    printf("Enter travel time (HHMM) ex: 0930 : ");
    scanf("%d", &travelTime);

    srcIndex = findStationIndex(srcName);
    destIndex = findStationIndex(destName);

    if (srcIndex == -1 || destIndex == -1) {
        printf("Station not found. Check spelling!\n");
        return;
    }

    dijkstra(srcIndex, destIndex, travelTime);
}

int main() {
    int choice, i, j;

    // Combine all stations into allStations array
    for (i = 0; i < redCount; i++) strcpy(allStations[n++], redLine[i]);
    for (i = 0; i < blueCount; i++) {
        int exists = 0;
        for (j = 0; j < n; j++) {
            if (strcmp(blueLine[i], allStations[j]) == 0) { exists = 1; break; }
        }
        if (!exists) strcpy(allStations[n++], blueLine[i]);
    }
    for (i = 0; i < greenCount; i++) {
        int exists = 0;
        for (j = 0; j < n; j++) {
            if (strcmp(greenLine[i], allStations[j]) == 0) { exists = 1; break; }
        }
        if (!exists) strcpy(allStations[n++], greenLine[i]);
    }

    // Initialize graph
    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++)
            graph[i][j] = 0;

    // Add connections (consecutive stations 1 km apart)
    for (i = 0; i < redCount-1; i++) {
        int u = findStationIndex(redLine[i]);
        int v = findStationIndex(redLine[i+1]);
        graph[u][v] = 1;
        graph[v][u] = 1;
    }
    for (i = 0; i < blueCount-1; i++) {
        int u = findStationIndex(blueLine[i]);
        int v = findStationIndex(blueLine[i+1]);
        graph[u][v] = 1;
        graph[v][u] = 1;
    }
    for (i = 0; i < greenCount-1; i++) {
        int u = findStationIndex(greenLine[i]);
        int v = findStationIndex(greenLine[i+1]);
        graph[u][v] = 1;
        graph[v][u] = 1;
    }

    // Menu-driven system
    do {
        printf("\n--- MetroConnect Menu ---\n");
        printf("1. View Metro Lines\n");
        printf("2. View Stations Line-wise\n");
        printf("3. Plan Trip (Shortest Path + Fare)\n");
        printf("4. Peak Hours Info\n");
        printf("5. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                printf("Red Line  : Miyapur ↔ LB Nagar\n");
                printf("Blue Line : Nagole ↔ Raidurg\n");
                printf("Green Line: JBS Parade Ground ↔ MGBS\n");
                break;
            case 2:
                displayStations("Red Line", redLine, redCount);
                displayStations("Blue Line", blueLine, blueCount);
                displayStations("Green Line", greenLine, greenCount);
                break;
            case 3:
                planTrip();
                break;
            case 4:
                printf("Peak Hours: 9:00-10:30 AM & 16:30-18:00 PM\n");
                printf("Extra fare during peak: Rs. 10\n");
                break;
            case 5:
                printf("Exiting MetroConnect. Have a safe journey!\n");
                break;
            default:
                printf("Invalid choice! Try again.\n");
        }

    } while(choice != 5);

    return 0;
}
