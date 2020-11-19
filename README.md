import java.io.*;
import javax.swing.JOptionPane;

public class Vehiculo {

    public static void crearArchivo(String nomArchivo) throws IOException{
        String matricula;
        String modelo;
        String marca;
        double precio;
        String resp;

        //objeto para guardar archivos binarios, en el objeto FileOutputStream se pone 
        //un segundo valor "true" para indicar que se tiene que escribir al final del archivo
        DataOutputStream arch= new DataOutputStream(new FileOutputStream(nomArchivo,true));

        do { 
            matricula=JOptionPane.showInputDialog("Matricula: ");
            modelo=JOptionPane.showInputDialog("Modelo: ");
            marca=JOptionPane.showInputDialog("Marca: ");
            precio=Double.parseDouble(JOptionPane.showInputDialog("Precio: "));
            
            arch.writeUTF(matricula);
            arch.writeUTF(modelo);
            arch.writeUTF(marca);
            arch.writeDouble(precio);

            do {
                resp=JOptionPane.showInputDialog("Desea escribir otro registro (S/N)");
            } while (!(resp.equals("s")||resp.equals("S")||resp.equals("n")||resp.equals("N")));

        } while (resp.equals("S")||resp.equals("s"));
        arch.close();
    }
    

    public static void main(String[] args) {
        
        try  {
            Vehiculo.crearArchivo("vehiculos");
        } catch (Exception e) {
            System.out.println("ERROR:" + e.toString());
        }
        try {
            LeerArchivo.leeArchivoBin("vehiculos");
        } catch (Exception e) {}
        
    }
}

class LeerArchivo {

    public static void leeArchivoBin(String nomArch) throws IOException{

        DataInputStream arch= new DataInputStream(new FileInputStream(nomArch));
        String matricula;
        String modelo;
        String marca;
        double precio;
        int reg=0;

        try {
            while (true) {
                matricula=arch.readUTF();
                modelo=arch.readUTF();
                marca=arch.readUTF();
                precio=arch.readDouble();

                reg++;  
                String s= new String("Registro "+reg+":"+"\nMatricula: "+matricula+"\nModelo: "+modelo+"\nMarca: "+marca+"\nPrecio: "+precio);
                JOptionPane.showMessageDialog(null,s);

            }
        } catch (EOFException e) {}
        arch.close();
    }
}
