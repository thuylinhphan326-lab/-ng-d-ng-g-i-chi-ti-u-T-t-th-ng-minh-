function formatMoney(num) {
    return num.toLocaleString("vi-VN") + " VNĐ";
}

function createPlan() {
    let money = Number(document.getElementById("moneyInput").value);

    // 🔥 TỰ ĐỘNG LÀM TRÒN NGHÌN
    money = Math.floor(money / 1000) * 1000;

    if (!money || money <= 0) {
        alert("Vui lòng nhập số tiền hợp lệ!");
        return;
    }

    // % KHÔNG TRÙNG NHAU
    let categories = {
        "Chi tiêu gia đình & chuẩn bị Tết": 0.22,
        "Quà biếu & thăm hỏi": 0.18,
        "Lì xì Tết": 0.20,
        "Trang phục & cá nhân": 0.15,
        "Ăn uống – đi chơi": 0.10,
        "Học tập đầu năm": 0.08,
        "Quỹ dự phòng": 0.07
    };

    let planHTML = "";

    for (let [name, percent] of Object.entries(categories)) {
        let amount = Math.floor((money * percent) / 1000) * 1000;
        let items = getItems(name, amount);

        planHTML += `
            <div class="card">
                <h2>${name} — ${formatMoney(amount)}</h2>
                <ul>${items.join("")}</ul>
            </div>
        `;
    }

    document.getElementById("plan").innerHTML = planHTML;
}

function getItems(category, total) {
    let ratios = {};

    // --------------------------
    // 🏡 1. Chi tiêu gia đình
    // --------------------------
    if (category === "Chi tiêu gia đình & chuẩn bị Tết") {
        ratios = {
            "Bánh kẹo Tết": 0.17,
            "Hoa – cây cảnh": 0.19,
            "Mâm ngũ quả": 0.13,
            "Trang trí nhà": 0.23,
            "Dọn dẹp – vệ sinh": 0.16,
            "Đồ dùng bếp": 0.12
        };
    }

    // 🎁 Quà biếu
    else if (category === "Quà biếu & thăm hỏi") {
        ratios = {
            "Biếu bố mẹ": 0.33,
            "Biếu ông bà": 0.26,
            "Biếu họ hàng": 0.20,
            "Biếu thầy cô": 0.13,
            "Tiền thăm hỏi": 0.08
        };
    }

    // 🧧 Lì xì
    else if (category === "Lì xì Tết") {
        ratios = {
            "Lì xì trẻ em": 0.28,
            "Lì xì anh chị em": 0.18,
            "Lì xì bố mẹ": 0.32,
            "Lì xì bạn bè": 0.15,
            "Lì xì phát sinh": 0.07
        };
    }

    // 👗 Trang phục
    else if (category === "Trang phục & cá nhân") {
        ratios = {
            "Quần áo mới": 0.36,
            "Giày dép": 0.27,
            "Làm tóc": 0.18,
            "Skincare – mỹ phẩm": 0.11,
            "Phụ kiện": 0.08
        };
    }

    // 🍜 Ăn uống – đi chơi
    else if (category === "Ăn uống – đi chơi") {
        ratios = {
            "Café – trà sữa": 0.18,
            "Đi chơi với bạn bè": 0.22,
            "Xem phim": 0.14,
            "Hội hoa xuân": 0.16,
            "Tiền xăng – xe": 0.15,
            "Quà lưu niệm": 0.15
        };
    }

    // 📚 Học tập đầu năm
    else if (category === "Học tập đầu năm") {
        ratios = {
            "Dụng cụ học tập": 0.42,
            "Sách mới": 0.28,
            "Ốp/kính điện thoại": 0.17,
            "In ấn – tài liệu": 0.13
        };
    }

    // 🛡 Quỹ dự phòng
    else if (category === "Quỹ dự phòng") {
        ratios = {
            "Phát sinh bất ngờ": 0.58,
            "Quỹ khẩn cấp": 0.42
        };
    }

    // Tính từng mục
    let list = [];
    for (let [name, r] of Object.entries(ratios)) {
        let money = Math.floor((total * r) / 1000) * 1000;
        list.push(`<li>${name} — ${formatMoney(money)}</li>`);
    }

    return list;
}
