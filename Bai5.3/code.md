using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;
namespace WinFormsApp2
{
    public partial class Form1 : Form
    {
        private BindingList<Product> productList = new BindingList<Product>();
        public Form1()
        {
            InitializeComponent();
        }
        private void Form5_3_Load(object sender, EventArgs e)
        {
            cboCategory.Items.AddRange(new string[] { "Điện thoại", "Laptop", "Phụ kiện" });
            cboCategory.SelectedIndex = 0;

            productList.Add(new Product("SP01", "iPhone 15", 22000000, 10, "Điện thoại"));
            productList.Add(new Product("SP02", "Dell XPS 13", 35000000, 5, "Laptop"));

            dgvProducts.DataSource = productList;
        }

        private void btnAdd_Click(object sender, EventArgs e)
        {
            if (string.IsNullOrWhiteSpace(txtProductId.Text) || string.IsNullOrWhiteSpace(txtProductName.Text))
            {
                MessageBox.Show("Vui lòng nhập đầy đủ Mã và Tên sản phẩm!", "Cảnh báo", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                return;
            }

            Product p = new Product
            {
                ProductId = txtProductId.Text,
                ProductName = txtProductName.Text,
                UnitPrice = decimal.TryParse(txtUnitPrice.Text, out decimal price) ? price : 0,
                Quantity = int.TryParse(txtQuantity.Text, out int qty) ? qty : 0,
                Category = cboCategory.SelectedItem.ToString()
            };

            productList.Add(p);
            ClearInputs();
        }

        private void dgvProducts_CellClick(object sender, DataGridViewCellEventArgs e)
        {
            if (e.RowIndex >= 0 && e.RowIndex < dgvProducts.Rows.Count)
            {
                DataGridViewRow row = dgvProducts.Rows[e.RowIndex];
                txtProductId.Text = row.Cells["ProductId"].Value?.ToString();
                txtProductName.Text = row.Cells["ProductName"].Value?.ToString();
                txtUnitPrice.Text = row.Cells["UnitPrice"].Value?.ToString();
                txtQuantity.Text = row.Cells["Quantity"].Value?.ToString();
                cboCategory.SelectedItem = row.Cells["Category"].Value?.ToString();
            }
        }

        private void btnEdit_Click(object sender, EventArgs e)
        {
            Product selected = productList.FirstOrDefault(p => p.ProductId == txtProductId.Text);
            if (selected != null)
            {
                selected.ProductName = txtProductName.Text;
                selected.UnitPrice = decimal.TryParse(txtUnitPrice.Text, out decimal price) ? price : 0;
                selected.Quantity = int.TryParse(txtQuantity.Text, out int qty) ? qty : 0;
                selected.Category = cboCategory.SelectedItem?.ToString();
                dgvProducts.Refresh();
            }
        }

        private void btnDelete_Click(object sender, EventArgs e)
        {
            if (dgvProducts.CurrentRow != null)
            {
                string id = dgvProducts.CurrentRow.Cells["ProductId"].Value.ToString();
                DialogResult dialog = MessageBox.Show($"Bạn có chắc muốn xóa sản phẩm {id}?", "Xác nhận", MessageBoxButtons.YesNo, MessageBoxIcon.Question);
                if (dialog == DialogResult.Yes)
                {
                    Product itemToRemove = productList.FirstOrDefault(p => p.ProductId == id);
                    if (itemToRemove != null)
                    {
                        productList.Remove(itemToRemove);
                        ClearInputs();
                    }
                }
            }
        }

        private void btnSearch_Click(object sender, EventArgs e)
        {
            string keyword = txtSearch.Text.ToLower().Trim();
            var filteredList = productList.Where(p => p.ProductName.ToLower().Contains(keyword)).ToList();
            dgvProducts.DataSource = new BindingList<Product>(filteredList);
        }

        private void ClearInputs()
        {
            txtProductId.Clear();
            txtProductName.Clear();
            txtUnitPrice.Clear();
            txtQuantity.Clear();
        }
    }
}
