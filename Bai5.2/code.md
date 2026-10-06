using System;
using System.Collections.Generic;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WinFormsApp1
{
    public partial class Form1 : Form
    {
        public class ServiceItem
        {
            public string Name { get; set; }
            public decimal Price { get; set; }

            public ServiceItem(string name, decimal price)
            {
                Name = name;
                Price = price;
            }

            public override string ToString()
            {
                return $"{Name} - {Price:N0} VNĐ";
            }
        }

        private Dictionary<string, List<ServiceItem>> serviceData = new Dictionary<string, List<ServiceItem>>();
        public Form1()
        {
            InitializeComponent();
        }
        private void Form5_2_Load(object sender, EventArgs e)
        {
            serviceData["Khám bệnh"] = new List<ServiceItem>
            {
                new ServiceItem("Khám tổng quát", 150000),
                new ServiceItem("Khám chuyên khoa", 250000)
            };
            serviceData["Xét nghiệm"] = new List<ServiceItem>
            {
                new ServiceItem("Xét nghiệm máu", 200000),
                new ServiceItem("Xét nghiệm nước tiểu", 100000)
            };
            serviceData["Chụp X-Quang"] = new List<ServiceItem>
            {
                new ServiceItem("X-Quang Phổi", 180000),
                new ServiceItem("X-Quang Cột sống", 220000)
            };
            serviceData["Vắc-xin"] = new List<ServiceItem>
            {
                new ServiceItem("Vắc-xin Cúm", 300000),
                new ServiceItem("Vắc-xin Viêm gan B", 250000)
            };

            cboCategory.Items.AddRange(new string[] { "Khám bệnh", "Xét nghiệm", "Chụp X-Quang", "Vắc-xin" });
            cboCategory.SelectedIndex = 0;
        }

        private void cboCategory_SelectedIndexChanged(object sender, EventArgs e)
        {
            lstAvailableServices.Items.Clear();
            string selectedCat = cboCategory.SelectedItem.ToString();
            if (serviceData.ContainsKey(selectedCat))
            {
                foreach (var item in serviceData[selectedCat])
                {
                    lstAvailableServices.Items.Add(item);
                }
            }
        }

        private void btnSelect_Click(object sender, EventArgs e)
        {
            if (lstAvailableServices.SelectedItem != null)
            {
                ServiceItem selectedItem = (ServiceItem)lstAvailableServices.SelectedItem;
                lstSelectedServices.Items.Add(selectedItem);
                CalculateTotal(null, null);
            }
        }

        private void btnRemove_Click(object sender, EventArgs e)
        {
            if (lstSelectedServices.SelectedItem != null)
            {
                lstSelectedServices.Items.Remove(lstSelectedServices.SelectedItem);
                CalculateTotal(null, null);
            }
        }

        private void btnClearAll_Click(object sender, EventArgs e)
        {
            lstSelectedServices.Items.Clear();
            CalculateTotal(null, null);
        }

        private void CalculateTotal(object sender, EventArgs e)
        {
            decimal subTotal = 0;
            foreach (ServiceItem item in lstSelectedServices.Items)
            {
                subTotal += item.Price;
            }

            decimal discountRate = nudDiscount.Value;
            decimal total = subTotal * (1 - discountRate / 100);

            txtSubTotal.Text = subTotal.ToString("N0") + " VNĐ";
            txtTotal.Text = total.ToString("N0") + " VNĐ";
        }
    }
}
