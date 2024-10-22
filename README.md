# taitungmonkey.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>停機記錄表單</title>
    <style>
        body { font-family: Arial, sans-serif; }
        form { margin-bottom: 20px; }
        table { width: 100%; border-collapse: collapse; }
        table, th, td { border: 1px solid black; }
        th, td { padding: 10px; text-align: left; }
    </style>
</head>
<body>

    <h2>停機記錄</h2>
    <form id="downtimeForm">
        <label for="startTime">停機開始時間:</label>
        <input type="datetime-local" id="startTime" name="startTime" required><br><br>

        <label for="endTime">停機結束時間:</label>
        <input type="datetime-local" id="endTime" name="endTime" required><br><br>

        <label for="reason">原因:</label>
        <input type="text" id="reason" name="reason" required><br><br>

        <label for="station">站點:</label>
        <input type="text" id="station" name="station" required><br><br>

        <input type="submit" value="提交">
    </form>

    <h3>篩選記錄</h3>
    <label for="filterDate">選擇日期來篩選記錄:</label>
    <input type="date" id="filterDate" name="filterDate"><br><br>
    <button onclick="filterRecords()">篩選</button>

    <h3>已提交的停機記錄</h3>
    <table id="recordTable">
        <thead>
            <tr>
                <th>停機開始時間</th>
                <th>停機結束時間</th>
                <th>原因</th>
                <th>站點</th>
                <th>日期</th>
            </tr>
        </thead>
        <tbody>
            <!-- 提交的記錄會顯示在這裡 -->
        </tbody>
    </table>

    <script>
        const records = []; // 用來儲存所有提交的記錄

        document.getElementById('downtimeForm').addEventListener('submit', function(event) {
            event.preventDefault(); // 防止表單重新加載頁面

            // 獲取表單數據
            const startTime = document.getElementById('startTime').value;
            const endTime = document.getElementById('endTime').value;

            // 防呆檢查：確保結束時間不小於開始時間
            if (new Date(endTime) < new Date(startTime)) {
                alert("請選擇正確的時間，結束時間不能早於開始時間！");
                return; // 阻止表單提交
            }

            const reason = document.getElementById('reason').value;
            const station = document.getElementById('station').value;
            const date = startTime.split('T')[0]; // 提取日期

            // 保存數據到記錄陣列
            records.push({ startTime, endTime, reason, station, date });

            // 更新表格顯示所有記錄
            displayRecords(records);

            // 清空表單
            document.getElementById('downtimeForm').reset();
        });

        function displayRecords(recordsToDisplay) {
            const table = document.getElementById('recordTable').getElementsByTagName('tbody')[0];
            table.innerHTML = ''; // 清空表格

            recordsToDisplay.forEach(record => {
                const newRow = table.insertRow();

                const cell1 = newRow.insertCell(0);
                const cell2 = newRow.insertCell(1);
                const cell3 = newRow.insertCell(2);
                const cell4 = newRow.insertCell(3);
                const cell5 = newRow.insertCell(4);

                cell1.textContent = record.startTime;
                cell2.textContent = record.endTime;
                cell3.textContent = record.reason;
                cell4.textContent = record.station;
                cell5.textContent = record.date;
            });
        }

        function filterRecords() {
            const filterDate = document.getElementById('filterDate').value;

            if (filterDate) {
                // 根據選擇的日期來篩選記錄
                const filteredRecords = records.filter(record => record.date === filterDate);
                displayRecords(filteredRecords);
            } else {
                // 如果未選擇日期，顯示所有記錄
                displayRecords(records);
            }
        }
    </script>

</body>
</html>
