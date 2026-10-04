# Bài 1: Khôi phục commit đã mất bằng Git Reflog

## 1. Mục tiêu

Thực hành sử dụng Git Reflog để truy tìm và khôi phục một commit đã bị loại khỏi lịch sử branch sau khi thực hiện `git reset --hard`.

## 2. Khởi tạo Repository

Khởi tạo Git Repository:

```bash
git init
```

Cấu hình danh tính Git ở cấp độ local:

```bash
git config --local user.name "Cao Chí Thiện"
git config --local user.email "cct20062250@gmail.com"
```

Kiểm tra cấu hình:

```bash
git config --local --list
```

Kết quả cho thấy `user.name` và `user.email` được cấu hình ở cấp độ local.

## 3. Tạo commit ban đầu

Tạo file `README.md`, sau đó đưa file vào Staging Area và tạo commit:

```bash
git add README.md
git commit -m "Initial commit"
```

Commit ban đầu:

```text
bc9b794 Initial commit
```

## 4. Tạo commit chứa tính năng quan trọng

Tạo file `feature.txt` với nội dung:

```text
Day la tinh nang quan trong
```

Đưa file vào Staging Area:

```bash
git add .
```

Tạo commit:

```bash
git commit -m "Them tinh nang quan trong"
```

Commit chứa tính năng quan trọng:

```text
96dffdc Them tinh nang quan trong
```

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả:

```text
96dffdc (HEAD -> master) Them tinh nang quan trong
bc9b794 Initial commit
```

## 5. Giả lập việc làm mất commit

Thực hiện lệnh:

```bash
git reset --hard HEAD~1
```

Git đưa `HEAD` trở về commit trước đó:

```text
HEAD is now at bc9b794 Initial commit
```

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả:

```text
bc9b794 (HEAD -> master) Initial commit
```

Commit `96dffdc Them tinh nang quan trong` không còn xuất hiện trong `git log`.

Kiểm tra file:

```powershell
Test-Path feature.txt
```

Kết quả:

```text
False
```

Điều này cho thấy commit chứa tính năng quan trọng đã bị loại khỏi lịch sử branch và file `feature.txt` cũng không còn trong working directory.

## 6. Sử dụng Reflog để tìm commit đã mất

Thực hiện:

```bash
git reflog
```

Kết quả:

```text
bc9b794 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
96dffdc HEAD@{1}: commit: Them tinh nang quan trong
bc9b794 (HEAD -> master) HEAD@{2}: commit (initial): Initial commit
```

Từ Reflog xác định được hash của commit đã mất:

```text
96dffdc
```

## 7. Khôi phục commit

Sử dụng hash tìm được từ Reflog:

```bash
git reset --hard 96dffdc
```

Kết quả:

```text
HEAD is now at 96dffdc Them tinh nang quan trong
```

## 8. Kiểm tra commit đã khôi phục

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả:

```text
96dffdc (HEAD -> master) Them tinh nang quan trong
bc9b794 Initial commit
```

Commit `Them tinh nang quan trong` đã xuất hiện trở lại.

Kiểm tra nội dung file:

```powershell
Get-Content feature.txt
```

Kết quả:

```text
Day la tinh nang quan trong
```

Kiểm tra file tồn tại:

```powershell
Test-Path feature.txt
```

Kết quả:

```text
True
```

## 9. Kiểm tra trạng thái Repository

Thực hiện:

```bash
git status
```

Kết quả:

```text
On branch master
nothing to commit, working tree clean
```

## 10. Reflog sau khi khôi phục

Thực hiện:

```bash
git reflog
```

Kết quả:

```text
96dffdc (HEAD -> master) HEAD@{0}: reset: moving to 96dffdc
bc9b794 HEAD@{1}: reset: moving to HEAD~1
96dffdc (HEAD -> master) HEAD@{2}: commit: Them tinh nang quan trong
bc9b794 HEAD@{3}: commit (initial): Initial commit
```

Kết quả cho thấy Reflog đã ghi lại toàn bộ quá trình di chuyển của `HEAD`, bao gồm cả commit đã bị reset.

## 11. Kết luận

Đã khôi phục thành công commit:

```text
96dffdc Them tinh nang quan trong
```

sau khi commit bị loại khỏi lịch sử branch bằng:

```bash
git reset --hard HEAD~1
```

Quá trình khôi phục được thực hiện hoàn toàn bằng Git Reflog:

1. Tạo commit chứa tính năng quan trọng.
2. Sử dụng `git reset --hard HEAD~1` để giả lập việc làm mất commit.
3. Kiểm tra `git log` và xác nhận commit không còn trong lịch sử hiện tại.
4. Sử dụng `git reflog` để tìm hash `96dffdc`.
5. Sử dụng `git reset --hard 96dffdc` để khôi phục commit.
6. Kiểm tra lại lịch sử và nội dung `feature.txt`.

Không viết lại mã nguồn thủ công trong quá trình khôi phục.