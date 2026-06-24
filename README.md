using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace BibliotecaApp
{
    public partial class FormCarte : Form
    {
        SqlConnection conn = new SqlConnection(
            @"Data Source=.;Initial Catalog=BibliotecaTEST;Integrated Security=True");

        public FormCarte()
        {
            InitializeComponent();
        }

        // CREATE
        private void btnAdd_Click(object sender, EventArgs e)
        {
            conn.Open();

            SqlCommand cmd = new SqlCommand(
                "INSERT INTO Carte VALUES(@id,@n,@e,@a)", conn);

            cmd.Parameters.AddWithValue("@id", txtID.Text);
            cmd.Parameters.AddWithValue("@n", txtNume.Text);
            cmd.Parameters.AddWithValue("@e", txtEditie.Text);
            cmd.Parameters.AddWithValue("@a", txtAn.Text);

            cmd.ExecuteNonQuery();
            conn.Close();

            MessageBox.Show("Carte adăugată!");
            LoadData();
        }

        // READ
        private void btnShow_Click(object sender, EventArgs e)
        {
            LoadData();
        }

        void LoadData()
        {
            conn.Open();

            SqlDataAdapter da = new SqlDataAdapter("SELECT * FROM Carte", conn);
            DataTable dt = new DataTable();
            da.Fill(dt);

            dgvCarte.DataSource = dt;

            conn.Close();
        }

        // UPDATE
        private void btnUpdate_Click(object sender, EventArgs e)
        {
            conn.Open();

            SqlCommand cmd = new SqlCommand(
                "UPDATE Carte SET NUME=@n, EDITIE=@e, ANUL=@a WHERE ID_CARTE=@id", conn);

            cmd.Parameters.AddWithValue("@id", txtID.Text);
            cmd.Parameters.AddWithValue("@n", txtNume.Text);
            cmd.Parameters.AddWithValue("@e", txtEditie.Text);
            cmd.Parameters.AddWithValue("@a", txtAn.Text);

            cmd.ExecuteNonQuery();
            conn.Close();

            MessageBox.Show("Carte modificată!");
            LoadData();
        }

        // DELETE
        private void btnDelete_Click(object sender, EventArgs e)
        {
            conn.Open();

            SqlCommand cmd = new SqlCommand(
                "DELETE FROM Carte WHERE ID_CARTE=@id", conn);

            cmd.Parameters.AddWithValue("@id", txtID.Text);

            cmd.ExecuteNonQuery();
            conn.Close();

            MessageBox.Show("Carte ștearsă!");
            LoadData();
        }

        // click pe tabel -> fill textbox
        private void dgvCarte_CellClick(object sender, DataGridViewCellEventArgs e)
        {
            txtID.Text = dgvCarte.CurrentRow.Cells[0].Value.ToString();
            txtNume.Text = dgvCarte.CurrentRow.Cells[1].Value.ToString();
            txtEditie.Text = dgvCarte.CurrentRow.Cells[2].Value.ToString();
            txtAn.Text = dgvCarte.CurrentRow.Cells[3].Value.ToString();
        }
    }
}
