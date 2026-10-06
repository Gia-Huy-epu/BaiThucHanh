using System;
using System.Collections.Generic;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;
namespace WinFormsApp3
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        private void btnRegister_Click(object sender, EventArgs e)
        {
            epCheck.Clear();
            bool isValid = true;

            if (string.IsNullOrWhiteSpace(txtUsername.Text))
            {
                epCheck.SetError(txtUsername, "Tên đăng nhập không được để trống!");
                isValid = false;
            }

            if (string.IsNullOrWhiteSpace(txtPassword.Text))
            {
                epCheck.SetError(txtPassword, "Mật khẩu không được để trống!");
                isValid = false;
            }

            if (txtConfirmPassword.Text != txtPassword.Text)
            {
                epCheck.SetError(txtConfirmPassword, "Mật khẩu xác nhận không khớp!");
                isValid = false;
            }

            int age = DateTime.Now.Year - dtpBirthDate.Value.Year;
            if (dtpBirthDate.Value.Date > DateTime.Now.AddYears(-age)) age--;

            if (age < 18)
            {
                epCheck.SetError(dtpBirthDate, "Độ tuổi phải từ 18 tuổi trở lên!");
                isValid = false;
            }

            if (!chkTerms.Checked)
            {
                epCheck.SetError(chkTerms, "Bạn phải đồng ý với Điều khoản dịch vụ!");
                isValid = false;
            }

            if (isValid)
            {
                MessageBox.Show("Đăng ký tài khoản thành công!", "Thông báo", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }
        }

        private void btnClear_Click(object sender, EventArgs e)
        {
            txtUsername.Clear();
            txtPassword.Clear();
            txtConfirmPassword.Clear();
            dtpBirthDate.Value = DateTime.Now;
            rdoMale.Checked = true;
            chkTerms.Checked = false;
            epCheck.Clear();
        }
    }
}
