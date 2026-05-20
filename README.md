import java.util.Scanner;

public class TarificadorTaxiYopal {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in); 

        int V, hora, P, edad;
        double km;
        char D, L, R;

        System.out.print("Vehiculo (1-3): ");
        V = sc.nextInt();
        if (V<1 || V>3){ System.out.println("Tipo de vehículo no válido"); }

        System.out.print("Km: ");
        km = sc.nextDouble();
        if (km<=0){ System.out.println("Distancia inválida"); }

        System.out.print("Hora: ");
        hora = sc.nextInt();
        if (hora<0 || hora>23){ System.out.println("Hora inválida"); }

        System.out.print("Dom/Fest (S/N): ");
        D = sc.next().charAt(0);
        System.out.print("Lluvia (S/N): ");
        L = sc.next().charAt(0);
        System.out.print("Rural (S/N): ");
        R = sc.next().charAt(0);

        boolean sn = ( (D=='S'||D=='N') && (L=='S'||L=='N') && (R=='S'||R=='N') );
        if (!sn){ System.out.println("Respuesta S/N inválida"); }

        System.out.print("Pasajero (1-4): ");
        P = sc.nextInt();
        if (P<1 || P>4){ System.out.println("Tipo de pasajero no válido"); }

        System.out.print("Edad: ");
        edad = sc.nextInt();
        if (edad<0 || edad>120){ System.out.println("Edad fuera de rango"); }

        double tkm=0; int tmin=0;
        if (V==1){ tkm=1200; tmin=5000; }
        else if (V==2){ tkm=2000; tmin=8000; }
        else { tkm=2800; tmin=12000; }

        double sub = km*tkm;
        boolean min1=false;
        if (sub<tmin){ sub=tmin; min1=true; }

        double rec=0;
        boolean noct = (hora>=22 || hora<5);
        if (noct) rec+=20;
        if (D=='S') rec+=15;
        if (L=='S') rec+=10;
        if (R=='S') rec+=25;

        double vrec = sub*(rec/100);
        double conRec = sub + vrec;

        double desc=0;
        if (P==1) desc=10;
        else if (P==2) desc=8;
        else if (P==3){
            if (edad>=60) desc=12;
            else{
                System.out.println("Inconsistencia: edad no corresponde a adulto mayor");
                P=4; desc=0;
            }
        }

        double vdesc = conRec*(desc/100);
        double total = conRec - vdesc;

        boolean solid = false;
        if (R=='N' && total<tmin){
            total=tmin;
            solid=true;
        }

        System.out.println("\n====== RECIBO ======\n");
        System.out.println("Subtotal: $"+Math.round(sub));
        System.out.println("Recargos: "+rec+"% → $"+Math.round(vrec));
        System.out.println("Con recargos: $"+Math.round(conRec));
        System.out.println("Descuento: "+desc+"% → $"+Math.round(vdesc));
        System.out.println("TOTAL: $"+Math.round(total));
         System.out.println("\n=====================\n");
        if (min1) System.out.println("Se aplicó tarifa mínima");
        if (solid) System.out.println("Se aplicó tarifa solidaria mínima");

        sc.close();
    }
}
