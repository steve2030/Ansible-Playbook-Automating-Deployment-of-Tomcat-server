import boto3

def lambda_handler(event, context):
    
    # Create S3 client
    s3_client = boto3.client('s3')
    
    # List all S3 buckets
    response = s3_client.list_buckets()
    
    # Check compliance for each bucket
    for bucket in response['Buckets']:
        
        # Check if the bucket has a website configuration
        website = s3_client.get_bucket_website(Bucket=bucket['Name'])
        if 'Error' in website:
            print(f"Bucket {bucket['Name']} is not compliant - website configuration is missing")
        
        # Check if the bucket has default encryption enabled
        encryption = s3_client.get_bucket_encryption(Bucket=bucket['Name'])
        if 'ServerSideEncryptionConfiguration' not in encryption:
            print(f"Bucket {bucket['Name']} is not compliant - default encryption is not enabled")
        
        # Check if the bucket has versioning enabled
        versioning = s3_client.get_bucket_versioning(Bucket=bucket['Name'])
        if 'Status' in versioning and versioning['Status'] != 'Enabled':
            print(f"Bucket {bucket['Name']} is not compliant - versioning is not enabled")
        
        # Check if the bucket has public access blocked
        acl = s3_client.get_bucket_acl(Bucket=bucket['Name'])
        for grant in acl['Grants']:
            if 'URI' in grant['Grantee'] and grant['Grantee']['URI'] == 'http://acs.amazonaws.com/groups/global/AllUsers':
                print(f"Bucket {bucket['Name']} is not compliant - public access is not blocked")
                break
        
        # Check if the bucket has logging enabled
        logging = s3_client.get_bucket_logging(Bucket=bucket['Name'])
        if 'LoggingEnabled' not in logging:
            print(f"Bucket {bucket['Name']} is not compliant - logging is not enabled")
    
    return {
        'statusCode': 200,
        'body': 'Compliance check completed'
    }
