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

