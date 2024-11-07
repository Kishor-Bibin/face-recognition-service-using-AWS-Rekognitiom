

# Face Recognition Service using AWS Rekognition

Welcome! This README outlines the steps I took to set up a face recognition service using **AWS Rekognition**. The process leverages **Amazon S3** for image storage, **AWS Lambda** for processing images, and **DynamoDB** for managing face data. I followed a YouTube tutorial by *Tech Raj*, which provided a clear, practical guide on implementing AWS Rekognition to recognize faces from images.

## Table of Contents
1. [Overview](#overview)
2. [Setup and Requirements](#setup-and-requirements)
3. [Steps to Build the Service](#steps-to-build-the-service)
4. [Testing and Results](#testing-and-results)
5. [Considerations and Tips](#considerations-and-tips)

## Overview

This service uses **AWS Rekognition** to analyze and recognize faces within a set of images. The process includes:
- Uploading images to an S3 bucket, which triggers a Lambda function.
- The Lambda function sends each image to Rekognition, where unique face representations or “face prints” are generated.
- Face prints are saved in **DynamoDB**, tagged with associated names.

This setup automates face recognition, storing faces in a way that allows for easy future comparisons and recognition.

## Setup and Requirements

### Prerequisites
- **AWS Account**: Ensure you have access to AWS services such as S3, Lambda, Rekognition, and DynamoDB.
- **AWS CLI**: Install and configure the AWS CLI on your local machine.

### Services Used
- **Amazon S3**: Stores images.
- **AWS Lambda**: Processes images as they’re uploaded.
- **Amazon Rekognition**: Analyzes faces in images and generates face prints.
- **DynamoDB**: Stores face IDs with corresponding names for future lookups.

## Steps to Build the Service

### 1. Create an S3 Bucket
   - Create an S3 bucket to store images. Ensure it’s in the same region as other AWS services.
   - Enable trigger notifications for **object creation** so each upload triggers the Lambda function.

### 2. Create a DynamoDB Table
   - Set up a DynamoDB table (e.g., `face_recognition`) with a primary key of `RekognitionId` to store unique face IDs and their associated names.

### 3. Set Up a Lambda Function
   - Configure a Lambda function to process images uploaded to S3 and trigger the face indexing process in Rekognition.
   - Assign the Lambda function a role with permissions for Rekognition, S3, and DynamoDB.



### 4. Set Up an S3 Trigger
   - Configure S3 to automatically trigger the Lambda function on image uploads.

### 5. Create a Rekognition Collection
   - Use the AWS CLI to create a collection in Rekognition where all face data is stored. This collection is essential for indexing and recognizing faces.
   - Run this command:
     ```bash
     aws rekognition create-collection --collection-id famouspersons
     ```

### 6. Add Metadata to Images (Optional)
   - Before uploading, add metadata (e.g., `fullname` as a key) to images, so the Lambda function can associate each face with a name.
   - This step is important for structured face data and easy retrieval during recognition.

## Testing and Results

1. **Upload Test Images**: I tested the setup by uploading images of public figures, e.g., Elon Musk, Bill Gates, and Sundar Pichai. Each upload automatically triggered the Lambda function, which processed the images and updated DynamoDB with face prints and names.
2. **Run Recognition**: After uploading and indexing several images for each individual, I tested the face recognition by uploading new images of the same individuals. The system accurately identified each person based on their indexed face print in Rekognition.

## Considerations and Tips

- **Dataset Size**: To improve recognition accuracy, use multiple images per individual. This enables Rekognition to build a robust face print.
- **Permissions**: Ensure your Lambda execution role has permissions for Rekognition, DynamoDB, and S3.
- **Testing**: To test, use unique faces to verify the system is creating and storing accurate face IDs in DynamoDB.

## Conclusion

Using AWS Rekognition and integrating it with other AWS services, I built a scalable, automated face recognition system. The process highlights AWS’s power in simplifying complex tasks and leveraging cloud resources to manage face recognition in real time. By indexing faces in DynamoDB, the system can recognize individuals in future images with impressive accuracy.

--- 
