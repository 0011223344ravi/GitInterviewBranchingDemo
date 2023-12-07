# GitInterviewBranchingDemo

git init

git remote add origin git URL

git add .

git commit -m "Initial commit"

git push -u origin master


package com.example.contentencoding;

import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.ResponseStatus;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.util.zip.GZIPOutputStream;

@RestController
public class TestController1 {

    @PostMapping("/your-endpoint")
    public ResponseEntity<byte[]> yourEndpoint(@RequestBody String requestBody) {
        byte[] compressedBody = compress(requestBody.getBytes());

        HttpHeaders headers = new HttpHeaders();
        headers.set(HttpHeaders.CONTENT_ENCODING, "gzip");

        return ResponseEntity
                .status(HttpStatus.OK)
                .headers(headers)
                .body(compressedBody);
    }

    // Method to compress byte array using GZIP
    private byte[] compress(byte[] data) {
        try (ByteArrayOutputStream byteStream = new ByteArrayOutputStream();
             GZIPOutputStream gzipStream = new GZIPOutputStream(byteStream)) {

            gzipStream.write(data);
            gzipStream.finish();

            return byteStream.toByteArray();
        } catch (IOException e) {
            // Handle compression failure
            e.printStackTrace();
            return new byte[0];
        }
    }
}
................................................................................





import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;

public class YourService {

    private final RestTemplate restTemplate;

    public YourService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public void callThirdPartyAPI() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Content-Encoding", "gzip"); // Set your desired encoding here

        // You can also set other headers if needed
        // headers.set("Accept", "application/json");

        HttpEntity<String> requestEntity = new HttpEntity<>(headers);

        ResponseEntity<String> responseEntity = restTemplate.exchange(
            "https://third-party-api.com/endpoint",
            HttpMethod.GET, // Change to your desired HTTP method
            requestEntity,
            String.class
        );

        // Process the response as needed
        String responseBody = responseEntity.getBody();
        // Handle the response...
    }
}

