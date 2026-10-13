# Bài 1 - Session 12: CI đa phiên bản JDK (Matrix Strategy)

## Nội dung

Workflow `.github/workflows/ci-matrix.yml` chạy khi push vào nhánh `main`. Job `test-matrix` dùng `strategy.matrix` với `java-version: ['17', '21']`, nên GitHub tự tách thành 2 job chạy song song: `test-matrix (17)` và `test-matrix (21)`.

Mỗi job thực hiện: checkout (`actions/checkout@v5`), cài Java Temurin (`actions/setup-java@v5`, `cache: 'gradle'`), cấp quyền cho `gradlew`, rồi chạy `./gradlew test`.

## Lưu ý về build.gradle

Project cũ khai báo `toolchain { languageVersion = 17 }`. Với cấu hình đó, job JDK 21 sẽ đòi tìm JDK 17 để biên dịch và có thể báo lỗi toolchain. Bài này dùng `sourceCompatibility` và `targetCompatibility` bằng 17, nên cả JDK 17 lẫn 21 đều biên dịch được.

## Chuẩn bị và đẩy code

Thư mục này cần có `gradlew`. Nếu chưa có, tạo bằng Gradle trên máy (cần JDK 17):

```
gradle wrapper
```

Kiểm tra test chạy được ở máy:

```
./gradlew test
```

Tạo repo mới trên GitHub (ví dụ `IT209-SS12-BT1`), rồi đẩy code:

```
git init
git add .
git commit -m "Add matrix CI"
git branch -M main
git remote add origin https://github.com/fisher9126/IT209-SS12-BT1.git
git push -u origin main
```

## Kiểm tra

Vào tab Actions của repo, mở workflow "CI Matrix". Job `test-matrix` phải rẽ thành 2 job `test-matrix (17)` và `test-matrix (21)` chạy song song, cả hai đều tích xanh. Chụp ảnh màn hình trang này để nộp.

## Sản phẩm nộp

- `.github/workflows/ci-matrix.yml`
- Ảnh chụp tab Actions hiển thị luồng Matrix Test
