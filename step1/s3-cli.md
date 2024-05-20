# S3 버킷 생성

aws s3api create-bucket --bucket wsu-sw-00-s3 --region ap-northeast-2 --create-bucket-configuration LocationConstraint=ap-northeast-2

# - 정적 웹사이트 호스팅 활성화

aws s3 website s3://wsu-sw-00-s3/ --index-document index.html


# - 퍼블릭 액세스 차단 해제

aws s3api delete-public-access-block --bucket wsu-sw-00-s3

# - 버킷 정책 설정

echo '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::wsu-sw-00-s3/*"
        }
    ]
}' > bucket-policy.json
aws s3api put-bucket-policy --bucket wsu-sw-00-s3 --policy file://bucket-policy.json

# - index.html 파일 업로드

aws s3 cp index.html s3://wsu-sw-00-s3/

# - 웹사이트 URL

echo "http://wsu-sw-00-s3.s3-website.ap-northeast-2.amazonaws.com"


# 삭제방법

# - 버킷의 모든 객체 삭제

aws s3 rm s3://wsu-sw-00-s3 --recursive

# - S3 버킷 삭제

aws s3api delete-bucket --bucket wsu-sw-00-s3 --region ap-northeast-2
