<template>
    <div class="card border-top border-0 border-3 border-info">
        <div class="card-body">
            <div class="row align-items-center">
                <div class="col-lg-4 col-md-12">
                    <label for="">Tìm kiếm</label>
                    <div class="input-group mt-2 mb-2">
                        <input type="text" class="form-control" placeholder="Tìm kiếm đơn đặt..." />
                        <button class="btn btn-primary">Tìm Kiếm</button>
                    </div>
                </div>
                <div class="col-lg-3 col-md-6">
                    <label for="">Từ ngày</label>
                    <input type="date" class="form-control mt-2 mb-2 w-100">
                </div>
                <div class="col-lg-3 col-md-6">
                    <label for="">Đến ngày</label>
                    <input type="date" class="form-control mt-2 mb-2">
                </div>
                <div class="col-lg-2 col-md-12">
                    <label for="">&nbsp;</label>
                    <button class="btn btn-primary w-100">Lọc</button>
                </div>
            </div>
        </div>
    </div>
    <div class="card">
        <div class="card-header">
            <h5 class="mt-2">DANH SÁCH ĐƠN ĐẶT HÀNG</h5>
        </div>
        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-bordered table-hover">
                    <thead class="align-middle">
                        <tr class="text-center table-primary">
                            <th>Mã đơn</th>
                            <th>Thời Gian Đặt Hàng</th>
                            <th>Quán Ăn</th>
                            <th>Khách Hàng</th>
                            <th>Người Nhận</th>
                            <th>Shipper</th>
                            <th>Tiền Hàng</th>
                            <th>Phí Ship</th>
                            <th>Tổng Tiền</th>
                            <th>Trạng thái</th>
                            <th>Chi Tiết Đơn Đặt</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <template v-for="(value, index) in list_don_hang" :key="index">
                            <tr class="align-middle">
                                <td class="align-middle text-center">{{ value.ma_don_hang }}</td>
                                <td class="align-middle text-center">{{ value.created_at }}</td>
                                <td class="align-middle">{{ value.ten_quan_an }}</td>
                                <td class="align-middle">{{ value.ho_va_ten_khach_hang }}</td>
                                <td class="align-middle">{{ value.ten_nguoi_nhan }}</td>
                                <td class="align-middle">{{ value.ho_va_ten_shipper ? value.ho_va_ten_shipper : "-" }}
                                </td>
                                <td class="align-middle text-end">{{ formatVND(value.tien_hang) }}</td>
                                <td class="align-middle text-end">{{ formatVND(value.phi_ship) }}</td>
                                <td class="align-middle text-end">{{ formatVND(value.tong_tien) }}</td>
                                <td class="align-middle text-center">
                                    <button v-if="value.tinh_trang == 0" class="btn btn-warning btn-sm w-100">Chờ xác
                                        nhận</button>
                                    <button v-if="value.tinh_trang == 1" class="btn btn-primary btn-sm w-100">Shipper đã
                                        nhận đơn</button>
                                    <button v-if="value.tinh_trang == 2" class="btn btn-info btn-sm w-100">Đang giao hàng</button>
                                    <button v-if="value.tinh_trang == 3" class="btn btn-success btn-sm w-100">Giao thành
                                        công</button>
                                    <button v-if="value.tinh_trang == 4" class="btn btn-danger btn-sm w-100">Đơn bị
                                        hủy</button>
                                </td>
                                <td class="align-middle text-center">
                                    <button v-on:click="xemChiTiet(value)" class="btn btn-info w-100 btn-sm me-2"
                                        data-bs-toggle='modal' data-bs-target='#chiTietModal' style="color: white;">
                                        <i class="fas fa-eye"></i> Chi tiết
                                    </button>
                                </td>
                                <td class="align-middle text-center">
                                    <button class="btn btn-success btn-sm me-2">
                                        <i class="fas fa-check"></i> Xác nhận
                                    </button>
                                    <button v-on:click="Object.assign(don_huy, value)" class="btn btn-danger btn-sm"
                                        data-bs-toggle='modal' data-bs-target='#huyModal'>
                                        <i class="fas fa-times"></i> Hủy
                                    </button>
                                </td>
                            </tr>
                        </template>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <div class='modal fade' id='chiTietModal' tabindex='-1' aria-labelledby='modalLabel' aria-hidden='true'>
        <div class='modal-dialog modal-lg'>
            <div class='modal-content'>
                <div class='modal-header'>
                    <h5 class='modal-title' id='modalLabel'>Chi Tiết Đơn Hàng</h5>
                    <button type='button' class='btn-close' data-bs-dismiss='modal' aria-label='Close'></button>
                </div>
                <div class='modal-body'>
                    <!-- Chi Tiết Sản Phẩm -->
                    <div class="table-responsive">
                        <table class="table table-hover table-bordered mb-0">
                            <thead class="">
                                <tr class="align-middle table-primary text-center">
                                    <th>Tên Món Ăn</th>
                                    <th>Số Lượng</th>
                                    <th>Đơn Giá</th>
                                    <th>Ghi Chú</th>
                                    <th>Thành Tiền</th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(value, index) in list_chi_tiet" :key="index">
                                    <tr class="align-middle">
                                        <td>{{ value.ten_mon_an }}</td>
                                        <td class="text-center">{{ value.so_luong }}</td>
                                        <td class="text-end">{{ formatVND(value.don_gia) }}</td>
                                        <td class="text-end">{{ value.ghi_chu ? value.ghi_chu : "-" }}</td>
                                        <td class="text-end fw-bold">{{ formatVND(value.thanh_tien) }}</td>
                                    </tr>
                                </template>
                            </tbody>
                        </table>
                    </div>
                </div>
                <div class='modal-footer'>
                    <button type='button' class='btn btn-secondary' data-bs-dismiss='modal'>Đóng</button>
                </div>
            </div>
        </div>
    </div>
    <div class="modal fade" id="huyModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Hủy Đơn Hàng</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-warning border-0 bg-warning alert-dismissible fade show py-2">
                        <div class="d-flex align-items-center">
                            <div class="font-35 text-dark"><i class="bx bx-info-circle"></i>
                            </div>
                            <div class="ms-3">
                                <h6 class="mb-0 text-dark">Cảnh Báo</h6>
                                <div class="text-dark">
                                    <span>Bạn muốn hủy đơn hàng <b>{{ don_huy.ma_don_hang }}</b> này</span> <br>
                                    <span>
                                        <b>Lưu ý:</b> Điều này không thể hoàn tác!
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                    <button type="button" class="btn btn-danger" v-on:click="huyDonHang()" data-bs-dismiss="modal">Xác
                        Nhận</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
            list_don_hang: [],
            list_chi_tiet: [],
            don_huy: {},
        };
    },
    mounted() {
        this.loadData();
    },
    methods: {
        formatVND(number) {
            return new Intl.NumberFormat('vi-VI', { style: 'currency', currency: 'VND' }).format(number,)
        },
        loadData() {
            axios
                .get('http://127.0.0.1:8000/api/admin/don-hang/data', {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("nhan_vien_login"),
                    },
                })
                .then((res) => {
                    this.list_don_hang = res.data.data;
                })
                .catch(res => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        xemChiTiet(payload) {
            axios
                .post("http://127.0.0.1:8000/api/admin/don-hang/data-chi-tiet", payload, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("nhan_vien_login"),
                    },
                })
                .then((res) => {
                    this.list_chi_tiet = res.data.data;
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                })
        },
        huyDonHang() {
            axios
                .post("http://127.0.0.1:8000/api/admin/don-hang/huy-don-hang", this.don_huy, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("nhan_vien_login"),
                    },
                })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                        this.loadData();
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                })
        }
    }
};
</script>

<style></style>