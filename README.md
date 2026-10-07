# Báo cáo kiểm thử API với Postman

**Sinh viên:** Nguyễn Trường Thành - 23010672

## 1. Giới thiệu
Postman là công cụ dùng để gửi request đến API và kiểm tra kết quả trả về. Báo cáo này trình bày quá trình thực hành các chức năng cơ bản của Postman trên API mẫu JSONPlaceholder.

## 2. Môi trường thực hiện
- Công cụ: Postman
- API: https://jsonplaceholder.typicode.com

![Giao diện Postman](images/anh1.png)

## 3. Nội dung thực hiện

### 3.1 Tạo Collection và Environment
Tạo collection `JSONPlaceholder Test` để gom các request, và environment `Dev` chứa biến `baseUrl` = `https://jsonplaceholder.typicode.com`. Các request dùng `{{baseUrl}}` thay vì gõ lại URL đầy đủ.

![Collection](images/anh2.png)
![Environment](images/anh3.png)

### 3.2 Request GET
Lấy toàn bộ danh sách bài viết tại `{{baseUrl}}/posts`, kết quả trả về 200 OK.

![GET all posts](images/anh4.png)

Lọc bài viết theo `userId=1` bằng Query Params, kết quả chỉ còn các bài của user 1.

![GET with params](images/anh5.png)

### 3.3 Request POST
Tạo bài viết mới bằng body JSON (raw), server trả về 201 Created kèm `id` mới.

![POST](images/anh6.png)

### 3.4 Request PUT và DELETE
PUT cập nhật bài viết số 1, trả về 200 OK.

![PUT](images/anh7.png)

DELETE xóa bài viết số 1, trả về 200 OK và body rỗng `{}`.

![DELETE](images/anh8.png)

> Lưu ý: JSONPlaceholder chỉ giả lập các thao tác tạo, sửa, xóa nên dữ liệu thực tế không bị thay đổi trên server.

### 3.5 Viết test script
Test cho request `Get all posts`:
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response là mảng và có dữ liệu", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an("array");
    pm.expect(jsonData.length).to.be.above(0);
});
```
![Test GET pass](images/anh9.png)

Test cho request `Create post`:
```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Title đúng như đã gửi", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.title).to.eql("Học Postman cơ bản");
});
```
![Test POST pass](images/anh10.png)

### 3.6 Collection Runner
Chạy toàn bộ collection: 5 request, 4 test, **Passed 4, Failed 0**.

![Runner](images/anh11.png)

### 3.7 File collection
File export: [my-collection.postman_collection.json](my-collection.postman_collection.json)

## 4. Tài liệu tham khảo
- Video hướng dẫn: https://www.youtube.com/watch?v=MFxk5BZulVU
- Postman Learning Center: https://learning.postman.com
- JSONPlaceholder: https://jsonplaceholder.typicode.com
