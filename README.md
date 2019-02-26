# Code example Simsimi
Main
```java
package Json;

import com.google.gson.Gson;
import entity.Sim;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;
import java.util.Scanner;

/**
 *
 * @author anhdt45
 */
public class Main {

    public static void main(String[] args) throws Exception {

        String key = "d0da4e85-3997-4d86-b61a-15329659c588";
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.print("Me: ");
            String text = scanner.nextLine();

            Main m = new Main();
            String encodeURL = m.getURL(key, URLEncoder.encode(text, "UTF-8"));

            Gson g = new Gson();
            Sim sim = g.fromJson(m.sendGet(encodeURL), Sim.class);

            if (sim.getResult() == 100) {
                System.out.println("Sim: " + sim.getResponse());
            } else {
                System.out.println("Sim: Hết lượt nói chuyện rồi bạn ơi.");
            }
        }

    }

    private String getURL(String key, String text) {
        String url = "http://sandbox.api.simsimi.com/request.p?key=" + key
                + "&lc=vn&ft=0.0&text=" + text;
        return url;
    }

    // HTTP GET request
    private String sendGet(String url) throws Exception {

        URL obj = new URL(url);
        HttpURLConnection con = (HttpURLConnection) obj.openConnection();

        // optional default is GET
        con.setRequestMethod("GET");

        //add request header
        con.setRequestProperty("User-Agent", "Mozilla/5.0");

        StringBuffer response;
        try (BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()))) {
            String inputLine;
            response = new StringBuffer();
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
        }

        //print result
        return response.toString();
    }
}
```
Sim.java
```java
public class Sim {
    private int result;
    private String response;
    private long id;
    private String msg;

    public int getResult() {
        return result;
    }

    public void setResult(int result) {
        this.result = result;
    }

    public String getResponse() {
        return response;
    }

    public void setResponse(String response) {
        this.response = response;
    }

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```
