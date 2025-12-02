// import java.util.Scanner;
// public class leakyBucket {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.print("Enter bucket size: ");
//         int size = sc.nextInt();
//         System.out.print("Enter leak rate: ");
//         int rate = sc.nextInt();
//         System.out.print("Enter number of cycles: ");
//         int cycles = sc.nextInt();
//         int bucket = 0;
//         while (cycles-- > 0) {
//             System.out.print("\nEnter packets arriving: ");
//             int in = sc.nextInt();
//             if (bucket + in > size) {
//                 int dropped = (bucket + in) - size;
//                 System.out.println("Bucket overflow! Packets dropped: " + dropped);
//                 bucket = size;  // bucket becomes full
//             } else {
//                 bucket += in;
//             }
//             System.out.println("Packets in bucket before leaking: " + bucket);
//             int sent = Math.min(bucket, rate);
//             bucket -= sent;
//             System.out.println("Packets sent (leaked): " + sent);
//             System.out.println("Packets left after leaking: " + bucket);
//         }
//         sc.close();
//     }
// }


// import java.util.Scanner;
// public class xor127 {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.print("Enter a string: ");
//         String str = sc.nextLine();
//         for (int i = 0; i < str.length(); i++) {
//             char result = (char)(str.charAt(i) ^ 0); 
//             System.out.print(result);
//         }
//         sc.close();
//     }
// }



// import java.util.Scanner;
// public class Bitwise127 {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.print("Enter a string: ");
//         String str = sc.nextLine();
//         System.out.println("\nOriginal String: " + str);
//         System.out.println("Character\tASCII\tAND(127)\tXOR(127)");
//         System.out.println("---------------------------------------------");
//         for (int i = 0; i < str.length(); i++) {
//             char ch = str.charAt(i);
//             int ascii = (int) ch;
//             int andResult = ch & 127;   
//             int xorResult = ch ^ 127;   
//             System.out.println("   " + ch + "\t\t" + ascii + "\t   " + andResult + "\t\t   " + xorResult);
//         }
//         sc.close();
//     }
// }



// import java.net.InetAddress;
// import java.util.Scanner;
// public class ping {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.print("Enter hostname: ");
//         String host = sc.next();
//         try {
//             InetAddress inet = InetAddress.getByName(host);
//             System.out.println("Pinging " + host);
//             boolean reachable = inet.isReachable(5000);
//             if (reachable) {
//                 System.out.println("Host is reachable");
//                 System.out.println("IP Address: " + inet.getHostAddress());
//             } else {
//                 System.out.println("Host is NOT reachable");
//             }
//         } catch (Exception e) {
//             System.out.println("Invalid Host");
//         }
//         sc.close();
//     }
// }


// import java.net.InetAddress;
// import java.net.UnknownHostException;
// import java.util.Scanner;
// public class DNS {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.println("Type a domain name to get IP address");
//         System.out.print("Domain: ");
//         String domain = sc.nextLine();
//         try {
//             InetAddress[] addresses = InetAddress.getAllByName(domain);
//             System.out.println("IP Addresses for " + domain + ":");
//             for (InetAddress addr : addresses) {
//                 System.out.println(" - " + addr.getHostAddress());
//             }

//         } catch (UnknownHostException e) {
//             System.out.println("Could not resolve domain: " + domain);
//         }

//         sc.close();
//     }
// }




// import java.util.*;
// public class Dijkstra {
//     static final int INF = 999;

//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);
//         System.out.print("Enter number of nodes: ");
//         int n = sc.nextInt();

//         int[][] g = new int[n][n];
//         System.out.println("Enter adjacency matrix (use 999 for no direct edge):");
//         for (int i = 0; i < n; i++){
//             for (int j = 0; j < n; j++){
//                 g[i][j] = sc.nextInt();
//             }
//         }

//         System.out.print("Enter source node (0 to " + (n - 1) + "): ");
//         int src = sc.nextInt();

//         int[] dist = new int[n];
//         boolean[] vis = new boolean[n];
//         Arrays.fill(dist, INF);
//         dist[src] = 0;

//         for (int k = 0; k < n - 1; k++) {
//             int u = -1, min = INF;
//             for (int i = 0; i < n; i++){
//                 if (!vis[i] && dist[i] < min) {
//                     min = dist[i];
//                     u = i;
//                 }
//             }
//             vis[u] = true;

//             for (int v = 0; v < n; v++){
//                 if (!vis[v] && g[u][v] != INF && dist[u] + g[u][v] < dist[v])
//                     dist[v] = dist[u] + g[u][v];
//             }
//         }

//         System.out.println("\nShortest distances from source node " + src + ":");
//         for (int i = 0; i < n; i++)
//             System.out.println("To node " + i + " = " + dist[i]);

//         sc.close();
//     }
// }




// import java.util.Scanner;

// public class DVR {
//     public static final int INF = 999;

//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);

//         System.out.print("Enter number of nodes: ");
//         int n = sc.nextInt();

//         int[][] cost = new int[n][n];
//         int[][] dist = new int[n][n];
//         int[][] nextHop = new int[n][n];

//         System.out.println("Enter Cost Matrix (use 999 for no direct link):");
//         for (int i = 0; i < n; i++) {
//             for (int j = 0; j < n; j++) {
//                 cost[i][j] = sc.nextInt();
//                 dist[i][j] = cost[i][j];

//                 if (i != j && cost[i][j] != INF)
//                     nextHop[i][j] = j;
//                 else
//                     nextHop[i][j] = -1;
//             }
//         }

//         boolean updated;
//         do {
//             updated = false;

//             for (int i = 0; i < n; i++) {
//                 for (int j = 0; j < n; j++) {
//                     for (int k = 0; k < n; k++) {
//                         if (dist[i][k] + cost[k][j] < dist[i][j]) {
//                             dist[i][j] = dist[i][k] + cost[k][j];
//                             nextHop[i][j] = nextHop[i][k];
//                             updated = true;
//                         }
//                     }
//                 }
//             }
//         } while (updated);

//         System.out.println("\nFinal Routing Table:");
//         for (int i = 0; i < n; i++) {
//             System.out.println("Router " + i + ":");
//             for (int j = 0; j < n; j++) {
//                 System.out.printf("To %d -> Cost: %d, NextHop: %d\n", j, dist[i][j], nextHop[i][j]);
//             }
//             System.out.println();
//         }

//         sc.close();
//     }
// }


// import java.util.Scanner;

// public class SubnetCalculator {
//     public static long ipToInt(String ip) {
//         String[] parts = ip.split("\\.");
//         long a = Integer.parseInt(parts[0]);
//         long b = Integer.parseInt(parts[1]);
//         long c = Integer.parseInt(parts[2]);
//         long d = Integer.parseInt(parts[3]);
//         return (a << 24) | (b << 16) | (c << 8) | d;
//     }
//     public static String intToIp(long ip) {
//         return ((ip >> 24) & 255) + "." +
//                ((ip >> 16) & 255) + "." +
//                ((ip >> 8) & 255) + "." +
//                (ip & 255);
//     }
//     public static long subnetMask(int prefix) {
//         return ~((1L << (32 - prefix)) - 1) & 0xFFFFFFFFL;
//     }

//     public static void main(String[] args) {

//         Scanner sc = new Scanner(System.in);

//         System.out.print("Enter IP address (ex: 192.168.1.0): ");
//         String ip = sc.nextLine();

//         System.out.print("Enter prefix length (ex: 24): ");
//         int prefix = sc.nextInt();

//         int newPrefix = prefix + 1;

//         long ipInt = ipToInt(ip);
//         long mask = subnetMask(prefix);
//         long newMask = subnetMask(newPrefix);

//         int hosts = (1 << (32 - newPrefix)) - 2;

//         System.out.println("\nNumber of subnets: 2");
//         System.out.println("Hosts per subnet: " + hosts);

//         for (int i = 0; i < 2; i++) {

//             long network = (ipInt & mask) | ((long) i << (32 - newPrefix));
//             long broadcast = network | (~newMask & 0xFFFFFFFFL);

//             long firstHost = network + 1;
//             long lastHost = broadcast - 1;

//             System.out.println("\nSubnet " + (i + 1) + ":");
//             System.out.println("Network Address: " + intToIp(network));
//             System.out.println("Broadcast Address: " + intToIp(broadcast));
//             System.out.println("Subnet Mask: " + intToIp(newMask));
//             System.out.println("First Host: " + intToIp(firstHost));
//             System.out.println("Last Host: " + intToIp(lastHost));
//         }
//     }
// }



// import java.util.*;

// public class GoBackN {
//     public static void main(String[] args) {
//         int totalFrames = 10;
//         int windowSize = 6;
//         int base = 0;
//         Random rand = new Random();

//         System.out.println("Go-BACK-N ARQ");
//         System.out.println("Total Frames = " + totalFrames + ", Window Size = " + windowSize);

//         while (base < totalFrames) {
//             System.out.println("\nSending Frames:");
//             for (int i = base; i < base + windowSize && i < totalFrames; i++) {
//                 System.out.println("Sent Frame " + i);
//             }

//             boolean ackLost = rand.nextInt(5) == 0; 

//             if (ackLost) {
//                 System.out.println("ACK for Frame " + base + " lost");
//                 System.out.println("Sender waits...");
//                 System.out.println("Sender resends from Frame " + base);
//             } else {
//                 System.out.println("ACK Received for Frame " + base);
//                 base++;
//                 System.out.println("New Base: " + base);
//             }
//         }

//         System.out.println("\nAll frames sent successfully!");
//     }
// }



// import java.util.*;

// public class Stuffing {
//     public static void main(String[] args) {
//         Scanner sc = new Scanner(System.in);

//         System.out.println("1. Character Stuffing");
//         System.out.println("2. Bit Stuffing");
//         System.out.print("Enter choice: ");
//         int ch = sc.nextInt();
//         sc.nextLine(); 

//         System.out.print("Enter data: ");
//         String data = sc.nextLine();

//         if (ch == 1) {
//             charStuff(data);
//         } else if (ch == 2) {
//             bitStuff(data);
//         } else {
//             System.out.println("Invalid choice");
//         }
//         sc.close();
//     }

//     static void charStuff(String data) {
//         // Enter data: ab$cd\ef
//         // Stuffed Data: ab\$cd\\ef$
//         char FLAG = '$', ESC = '\\';
//         System.out.print("Stuffed Data: ");
//         for (char c : data.toCharArray()) {
//             if (c == FLAG || c == ESC)
//                 System.out.print(ESC);
//             System.out.print(c);
//         }
//         System.out.println(FLAG);
//     }

//     static void bitStuff(String data) {
//         // Enter data: 11111011111
//         // Stuffed Data: 1111100111110
//         int count = 0;
//         System.out.print("Stuffed Data: ");
//         for (char c : data.toCharArray()) {
//             System.out.print(c);
//             if (c == '1') {
//                 count++;
//                 if (count == 5) {
//                     System.out.print("0");
//                     count = 0;
//                 }
//             } else {
//                 count = 0;
//             }
//         }
//         System.out.println();
//     }
// }


// public class CRC {
//     public static void main(String[] args) {
//         String data = "1101011011";         
//         String poly = "10011";              

//         String dividend = data;
//         for (int i = 0; i < poly.length()-1; i++) dividend += "0";

//         char[] msg = dividend.toCharArray();
//         char[] gen = poly.toCharArray();

//         for (int i = 0; i <= msg.length - gen.length; i++) {
//             if (msg[i] == '1') {
//                 for (int j = 0; j < gen.length; j++) {
//                     msg[i+j] = (msg[i+j] == gen[j]) ? '0' : '1';
//                 }
//             }
//         }
//         String crc = dividend.substring(dividend.length() - (poly.length() - 1));
//         System.out.println("Data: " + data);
//         System.out.println("CRC : " + crc);
//         System.out.println("Transmitted: " + data + crc);
//     }
// }
