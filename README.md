LEAKY BUCKET
-----------
import java.util.Scanner;
public class Leaky {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter bucket size: ");
        int size = sc.nextInt();
        System.out.print("Enter leak rate: ");
        int rate = sc.nextInt();
        System.out.print("Enter number of cycles: ");
        int cycles = sc.nextInt();
        int bucket = 0;
        while (cycles-- > 0) {
            System.out.print("\nEnter packets arriving: ");
            int in = sc.nextInt();
            if (bucket + in > size) {
                int dropped = (bucket + in) - size;
                System.out.println("Bucket overflow! Packets dropped: " + dropped);
                bucket = size;  // bucket becomes full
            } else {
                bucket += in;
            }
            System.out.println("Packets in bucket before leaking: " + bucket);
            int sent = Math.min(bucket, rate);
            bucket -= sent;
            System.out.println("Packets sent (leaked): " + sent);
            System.out.println("Packets left after leaking: " + bucket);
        }
        sc.close();
    }
}

XOR WITH 0
----------
import java.util.Scanner;
public class XorString {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a string: ");
        String str = sc.nextLine();
        for (int i = 0; i < str.length(); i++) {
            char result = (char)(str.charAt(i) ^ 0); // XOR with 0 does not change the character
            System.out.print(result);
        }
        sc.close();
    }
}

XOR , AND WITH 127
------------------
import java.util.Scanner;
public class Bitwise127 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a string: ");
        String str = sc.nextLine();
        System.out.println("\nOriginal String: " + str);
        System.out.println("Character\tASCII\tAND(127)\tXOR(127)");
        System.out.println("---------------------------------------------");
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            int ascii = (int) ch;
            int andResult = ch & 127;   // Bitwise AND
            int xorResult = ch ^ 127;   // Bitwise XOR
            System.out.println("   " + ch + "\t\t" + ascii + "\t   " +
                               andResult + "\t\t   " + xorResult);
        }
        sc.close();
    }
}

PING SERVICE
------------
import java.net.InetAddress;
import java.util.Scanner;
public class ping {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter hostname: ");
        String host = sc.next();
        try {
            InetAddress inet = InetAddress.getByName(host);
            System.out.println("Pinging " + host + "...");
            boolean reachable = inet.isReachable(5000); // 5 sec timeout
            if (reachable) {
                System.out.println("Host is reachable");
                System.out.println("IP Address: " + inet.getHostAddress());
            } else {
                System.out.println("Host is NOT reachable");
            }
        } catch (Exception e) {
            System.out.println("Invalid Host");
        }
        sc.close();
    }
}

DNS
---
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.util.Scanner;
public class SimpleDNS {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Welcome to Simple DNS Resolver!");
        System.out.println("Type a domain name to get its IP address.");
        System.out.println("Type 'exit' to stop the program.\n");
        while (true) {
            System.out.print("Domain: ");
            String domain = sc.nextLine();
            if (domain.equalsIgnoreCase("exit")) {
                System.out.println("Exiting DNS Resolver. Goodbye!");
                break;
            }
            try {
                // Resolve domain
                InetAddress[] addresses = InetAddress.getAllByName(domain);
                System.out.println("IP Addresses for " + domain + ":");
                for (InetAddress addr : addresses) {
                    System.out.println(" - " + addr.getHostAddress());
                }

            } catch (UnknownHostException e) {
                System.out.println("Could not resolve domain: " + domain);
            }
        }

        sc.close();
    }
}

DIJKSTRA
--------
import java.util.*;
public class Dijkstra {
    static final int INF = 999;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[][] g = new int[n][n];
        for (int i=0;i<n;i++)
            for (int j=0;j<n;j++)
                g[i][j]=sc.nextInt();
        int src = sc.nextInt();
        int[] dist = new int[n];
        boolean[] vis = new boolean[n];
        Arrays.fill(dist, INF);
        dist[src] = 0;
        for (int k=0;k<n-1;k++) {
            int u=-1, min=INF;
            for (int i=0;i<n;i++)
                if (!vis[i] && dist[i]<min){min=dist[i];u=i;}
            vis[u]=true;
            for (int v=0;v<n;v++)
                if (!vis[v] && g[u][v]!=INF && dist[u]+g[u][v]<dist[v])
                    dist[v]=dist[u]+g[u][v];
        }
        for (int i=0;i<n;i++)
            System.out.println(dist[i]);
    }
}

DVR
---
import java.util.Scanner;
public class Dvr {
    public static int INF = 999;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Number of Nodes:");
        int n = sc.nextInt();
        int [][]cost = new int[n][n];
        int [][]dist = new int[n][n];
        int [][]nextHop = new int[n][n];
        System.out.println("ENter the Cost Matrix:");
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                cost[i][j]=sc.nextInt();
                dist[i][j]=sc.nextInt();
                if(i!=j && cost[i][j] !=INF){
                    nextHop[i][j]=j;
                }
            }
        }
        boolean updated;
        do { 
            updated=false;
            for (int i = 0; i <n; i++) {
                for(int j=0; j<n;j++){
                    for(int k=0;k<n;k++){
                        if(dist[i][j]>cost[i][k]+dist[k][j]){
                            dist[i][j]=cost[i][k]+dist[k][j];
                            nextHop[i][j]= k;
                            updated=true;
                        }
                    }
                }
            }
        } while (updated);
        System.out.println("Final Table:");
        for(int i=0;i<n;i++){
            System.out.println("Router"+i+":");
            for(int j=0;j<n;j++){
                System.out.printf("To %d -> cost: %d, nexthop = %d\n ",j,dist[i][j],nextHop[i][j]);
            }
        }
    }
}

SUBNET AND BROADCAST TREE
-------------------------
import java.util.Scanner;

public class SimpleSubnetCalc {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter IP (e.g., 192.168.1.0): ");
        String ip = sc.nextLine();

        System.out.print("Enter prefix length (e.g., 26): ");
        int prefix = sc.nextInt();

        String[] parts = ip.split("\\.");
        int a = Integer.parseInt(parts[0]);
        int b = Integer.parseInt(parts[1]);
        int c = Integer.parseInt(parts[2]);
        int d = Integer.parseInt(parts[3]);

        // block size is 2^(8 - host bits)
        int blockSize = (int)Math.pow(2, 8 - (prefix - 24));

        System.out.println("\nSubnet block size = " + blockSize + " addresses");
        System.out.println("\nCalculated Subnets:");

        for (int i = 0; i < 256; i += blockSize) {
            int network = i;
            int broadcast = i + blockSize - 1;

            int firstHost = (blockSize == 1 ? network : network + 1);
            int lastHost  = (blockSize == 1 ? broadcast : broadcast - 1);

            System.out.println("\nSubnet:");
            System.out.println("Network Address: " + a + "." + b + "." + c + "." + network);
            System.out.println("Broadcast Address: " + a + "." + b + "." + c + "." + broadcast);
            System.out.println("First Host: " + a + "." + b + "." + c + "." + firstHost);
            System.out.println("Last Host: " + a + "." + b + "." + c + "." + lastHost);

            if (broadcast >= 255) break;
        }
    }
}



SLIDING WINDOW PROTOCOL+GO-BACK N
----------------------------------
import java.util.*;
class GoBackN{
 public static void main(String[] args){
 int totalFrames=10;
 int windowsize =4;
 int base =0;
 Random rand = new Random();
 System.out.println("Go-BACK-N");
 System.out.println("Total frames = 10, window size =4");
 while(base<totalFrames){
 System.out.println("Sending Frames");
 for(int i=base;i<base+windowsize && i<totalFrames;i++){
 System.out.println("Sent Frame "+i);
 }
 boolean acklost = rand.nextInt(5)==0;
 if(acklost){
 System.out.println("ack from frame "+base+" lost");
 System.out.println("sender waits");
 System.out.println("Sender resend from frame "+base);
 }else{
 System.out.println("Ack Received "+base);
 base++;
 System.out.println("New base: "+base);
 }
 }
 System.out.println("All frames sent successfully");
 }
}

CHAR AND BIT STUFFING
---------------------
import java.util.Scanner;

public class Stuffing {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("1. Character Stuffing");
        System.out.println("2. Bit Stuffing");
        System.out.print("Enter choice: ");
        int choice = sc.nextInt();
        sc.nextLine(); // clear buffer

        System.out.print("Enter data: ");
        String data = sc.nextLine();

        if (choice == 1) {
            charStuff(data);
        } else if (choice == 2) {
            bitStuff(data);
        } else {
            System.out.println("Invalid choice!");
        }

        sc.close();
    }

    // Character Stuffing
    static void charStuff(String data) {
        char FLAG = '$';  // frame delimiter
        char ESC = '\\';  // escape character

        System.out.print(FLAG); // start of frame

        for (char c : data.toCharArray()) {
            if (c == FLAG || c == ESC) {
                System.out.print(ESC); // escape FLAG or ESC
            }
            System.out.print(c);
        }

        System.out.println(FLAG); // end of frame
    }

    // Bit Stuffing
    static void bitStuff(String data) {
        String frameDelimiter = "01111110"; // commonly used
        System.out.print(frameDelimiter); // start of frame

        int count = 0;
        for (char c : data.toCharArray()) {
            System.out.print(c);
            if (c == '1') {
                count++;
                if (count == 5) {
                    System.out.print('0'); // stuff a 0 after five 1s
                    count = 0;
                }
            } else {
                count = 0;
            }
        }

        System.out.println(frameDelimiter); // end of frame
    }
}


CRC
---
import java.util.*;

public class SimpleCRC {

    // Performs XOR on the polynomial portion only
    static void xor(char[] dividend, String poly) {
        for (int i = 1; i < poly.length(); i++) {
            dividend[i] = (dividend[i] == poly.charAt(i)) ? '0' : '1';
        }
    }

    // Performs CRC division
    static String computeCRC(String data, String poly) {
        int n = poly.length();
        char[] temp = data.substring(0, n).toCharArray();
        int idx = n;

        while (idx < data.length()) {
            if (temp[0] == '1') {
                xor(temp, poly);
            }
            // Left shift
            for (int i = 0; i < n - 1; i++) {
                temp[i] = temp[i + 1];
            }
            // Add next data bit
            temp[n - 1] = data.charAt(idx);
            idx++;
        }

        // Final XOR
        if (temp[0] == '1') {
            xor(temp, poly);
        }

        return new String(temp).substring(1);    // remainder of (n-1) bits
    }


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter data to be transmitted: ");
        String data = sc.next();

        System.out.print("Enter generating polynomial: ");
        String poly = sc.next();

        int n = poly.length();

        // Append (n-1) zeros
        String paddedData = data + "0".repeat(n - 1);

        System.out.println("----------------------------------------");
        System.out.println("Padded data : " + paddedData);

        // Compute CRC
        String crc = computeCRC(paddedData, poly);
        System.out.println("CRC value   : " + crc);

        // Final data to send
        String finalData = data + crc;
        System.out.println("Final data to send: " + finalData);
        System.out.println("----------------------------------------");

        // Receiver side
        System.out.print("Enter received data: ");
        String recv = sc.next();

        String recvCRC = computeCRC(recv, poly);

        if (recvCRC.equals("0".repeat(n - 1))) {
            System.out.println("No error detected.");
        } else {
            System.out.println("Error detected!");
        }
    }
}

