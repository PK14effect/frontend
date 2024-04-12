# frontend
# 1. Auto Delete Todo List
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Move Items</title>
    <style>
        .container {
            display: flex;
            max-height: none;
            height: 90vh;
        }

        .column {
            flex: 1;
            border: 1px solid #ccc;
            margin: 5px;
            padding: 10px;
            max-height: 300px;
            overflow-y: auto;
            max-height: none;
            height: 90vh;
        }

        .item {
            background-color: #f0f0f0;
            border: 1px solid #aaa;
            padding: 5px;
            margin-bottom: 5px;
            cursor: pointer;
        }

        .STyl {
            text-align: center;
            background-color: white;
            border: 1px solid #eddede;
            font-family: system-ui;
            font-weight: 700;
            padding: 10px;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="column" id="items">
            <div class="STyl item fruit">Apple</div>
            <div class="STyl item vegetable">Broccoli</div>
            <div class="STyl item vegetable">Mushroom</div>
            <div class="STyl item fruit">Banana</div>
            <div class="STyl item vegetable">Tomato</div>
            <div class="STyl item fruit">Orange</div>
            <div class="STyl item fruit">Mango</div>
            <div class="STyl item fruit">Pineapple</div>
            <div class="STyl item vegetable">Cucumber</div>
            <div class="STyl item fruit">Watermelon</div>
            <div class="STyl item vegetable">Carrot</div>
        </div>
        <div class="column" id="fruits">
            <div style="
    font-family: system-ui;
    font-weight: 700;
    text-align: center;
    padding: 20px;
    background-color: #dddddd;
    margin-bottom: 10px;
">fruits</div>
        </div>
        <div class="column" id="vegetables">
            <div style="
    font-family: system-ui;
    font-weight: 700;
    text-align: center;
    padding: 20px;
    background-color: #dddddd;
    margin-bottom: 10px;
">vegetables</div>
        </div>
    </div>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function () {
            $(".item").click(function () {
                var $this = $(this);
                var parent = $this.parent();
                var type = $this.hasClass("fruit") ? "fruits" : "vegetables";

                if (parent.attr('id') === 'items') {
                    $this.appendTo("#" + type);
                } else {
                    $this.appendTo("#items");
                }

            });
        });
    </script>
</body>

</html>


# 2. Create data from API (OPTIONAL)
import axios from 'axios';

interface User {
    id: number;
    name: string;
    email: string;
    department: string;
}

async function fetchUsers(): Promise<User[]> {
    try {
        const response = await axios.get('https://dummyjson.com/users');
        return response.data;
    } catch (error) {
        console.error('Error fetching users:', error);
        return [];
    }
}

(async () => {
    const users = await fetchUsers();
    console.log(users);
})();
class Department {
    name: string;
    users: User[];

    constructor(name: string) {
        this.name = name;
        this.users = [];
    }

    addUser(user: User) {
        this.users.push(user);
    }
}

function groupUsersByDepartment(users: User[]): Department[] {
    const departmentsMap: { [key: string]: Department } = {};


    if (!Array.isArray(users)) {
        users = Object.values(users);
    }


    users.forEach(user => {
        if (!(user.department in departmentsMap)) {
            departmentsMap[user.department] = new Department(user.department);
        }
        departmentsMap[user.department].addUser(user);
    });


    return Object.values(departmentsMap);
}

(async () => {
    const users = await fetchUsers();
    const departments = groupUsersByDepartment(users);
    console.log(departments);
})();
