import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

public class MatchProgram2 extends JFrame {

    private JTextField txtEdadMin, txtEdadMax;
    private JComboBox<String> comboSexo;
    private JTextArea areaResultados;
    private JLabel lblFoto,lblEdadMin,lblEdadMax;

    private ArrayList<Miembro> miembros;

    public MatchProgram2() {
        setTitle("Match de Personas");
        setSize(450, 450);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(null);

        // ---- Inputs ----
        lblEdadMin = new JLabel("Edad mínima: ");
        txtEdadMin = new JTextField();
        lblEdadMin.setBounds(20, 20, 80, 25);
        txtEdadMin.setBounds(100, 20, 80, 25);
        add(lblEdadMin);
        add(txtEdadMin);

        lblEdadMax = new JLabel("Edad máxima: ");
        txtEdadMax = new JTextField();
        lblEdadMax.setBounds(20, 80, 80, 25); 
        txtEdadMax.setBounds(100, 80, 80, 25);
        add(lblEdadMax);
        add(txtEdadMax);

        comboSexo = new JComboBox<>(new String[]{"M", "F"});
        comboSexo.setBounds(200, 20, 80, 25);
        add(comboSexo);

        JButton btnBuscar = new JButton("Buscar");
        btnBuscar.setBounds(300, 20, 100, 25);
        add(btnBuscar);

        // ---- Área resultados ----
        areaResultados = new JTextArea();
        JScrollPane scroll = new JScrollPane(areaResultados);
        scroll.setBounds(20, 150, 200, 300);
        add(scroll);

        // ---- Imagen ----
        lblFoto = new JLabel();
        lblFoto.setBounds(250, 150, 150, 150);
        add(lblFoto);

        // ---- Datos ----
        miembros = generarMiembros();

        // ---- Evento ----
        btnBuscar.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                buscar();
            }
        });
    }

    // 🔍 Método de filtrado
    private ArrayList<Miembro> filtrar(
            ArrayList<Miembro> lista,
            int edadMin,
            int edadMax,
            String sexoBuscado) {

        ArrayList<Miembro> resultado = new ArrayList<>();

        for (Miembro m : lista) {
            if (m.getEdad() >= edadMin &&
                m.getEdad() <= edadMax &&
                m.getSexo().equalsIgnoreCase(sexoBuscado)) {

                resultado.add(m);
            }
        }

        return resultado;
    }

    // 🔍 Acción de búsqueda
    private void buscar() {
        try {
            int edadMin = Integer.parseInt(txtEdadMin.getText());
            int edadMax = Integer.parseInt(txtEdadMax.getText());
            String sexo = comboSexo.getSelectedItem().toString();

            ArrayList<Miembro> resultados =
                    filtrar(miembros, edadMin, edadMax, sexo);

            areaResultados.setText("");

            if (resultados.size() > 0) {
                for (Miembro m : resultados) {
                    areaResultados.append(m.toString() + "\n");
                }

                // Mostrar imagen del primer resultado
                mostrarImagen(resultados.get(0));
            } else {
                areaResultados.setText("No se encontraron coincidencias");
                lblFoto.setIcon(null);
            }

        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Ingrese edades válidas");
        }
    }

    // 🖼️ Mostrar imagen
    private void mostrarImagen(Miembro m) {
        String ruta = "/matchprogram2/img/" + m.getImagen();
        java.net.URL url = getClass().getResource(ruta);
        if (url != null) {
            ImageIcon icon = new ImageIcon(url);
            Image img = icon.getImage().getScaledInstance(200, 200, Image.SCALE_SMOOTH);
            lblFoto.setIcon(new ImageIcon(img));
        } else {
            System.out.println("No se encontró: " + ruta);
        }
    }

    // 🧪 Datos de prueba
    private ArrayList<Miembro> generarMiembros() {
        ArrayList<Miembro> lista = new ArrayList<>();

        lista.add(new Miembro("Ana", 25, "F", "ana.jpg"));
        lista.add(new Miembro("Luis", 30, "M", "luis.jpg"));
        lista.add(new Miembro("Maria", 22, "F", "maria.jpg"));
        lista.add(new Miembro("Carlos", 28, "M", "carlos.jpg"));
        lista.add(new Miembro("Sofia", 27, "F", "Sofia.jpg"));
        lista.add(new Miembro("Pedro", 35, "M", "pedro.jpg"));
        lista.add(new Miembro("Jorge",57,"M","jorge.jpg"));

        return lista;
    }

    public static void main(String[] args) {
        new MatchProgram2().setVisible(true);
    }
}
public class Miembro {
    private String nombre;
    private int edad;
    private String sexo;
    private String imagen;

    public Miembro(String nombre, int edad, String sexo, String imagen) {
        this.nombre = nombre;
        this.edad = edad;
        this.sexo = sexo;
        this.imagen = imagen;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    public String getSexo() { return sexo; }
    public String getRutaImagen() { return imagen; }
    public String getImagen() { 
        return imagen; 
    }
    @Override
    public String toString() {
        return nombre + " - Edad: " + edad + " - Sexo: " + sexo;
    }
}

