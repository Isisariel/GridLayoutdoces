import javax.swing;
import java.awt;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.image.BufferedImage;
import java.io.IOException;
import javax.imageio.ImageIO;

public class DocesGridLayout extends JFrame implements ActionListener {
    private JTextField[] quantidadeCampos;
    private JLabel[] docesLabel;
    private JLabel[] precoLabel;
    private JButton pedidoButton;
    private double[] precos = {3.50, 2.75, 1.00};

    public DocesGridLayout() {
        setTitle("Loja de doces da Ísis (GridLayout)");
        setSize(600, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new GridLayout(4, 3)); // 3 linhas de doces + 1 linha para o botão

        quantidadeCampos = new JTextField[3];
        docesLabel = new JLabel[3];
        precoLabel = new JLabel[3];

        String[] candyImagePaths = {"biscoito.jpeg", "Coracao.jpeg", "morango.jpeg"};
        for (int i = 0; i < 3; i++) {
            try {
                BufferedImage originalImage = ImageIO.read(getClass().getResource(candyImagePaths[i]));
                BufferedImage resizedImage = resizeImage(originalImage, 100, 100);
                ImageIcon candyIcon = new ImageIcon(resizedImage);
                docesLabel[i] = new JLabel(candyIcon);
                add(docesLabel[i]);

                precoLabel[i] = new JLabel("R$" + precos[i]);
                precoLabel[i].setHorizontalAlignment(JLabel.RIGHT);
                add(precoLabel[i]);

                quantidadeCampos[i] = new JTextField(5);
                add(quantidadeCampos[i]);

            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        pedidoButton = new JButton("Pedir");
        pedidoButton.addActionListener(this);
        add(new JLabel()); // Espaço vazio
        add(new JLabel()); // Espaço vazio
        add(pedidoButton);

        setVisible(true);
    }

    private BufferedImage resizeImage(BufferedImage originalImage, int targetWidth, int targetHeight) {
        BufferedImage resizedImage = new BufferedImage(targetWidth, targetHeight, BufferedImage.TYPE_INT_ARGB);
        Graphics2D g = resizedImage.createGraphics();
        g.drawImage(originalImage, 0, 0, targetWidth, targetHeight, null);
        g.dispose();
        return resizedImage;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == pedidoButton) {
            double total = 0;
            for (int i = 0; i < 3; i++) {
                try {
                    int quantity = Integer.parseInt(quantidadeCampos[i].getText());
                    total += precos[i] * quantity;
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(this, "Por favor, insira números válidos nas quantidades.");
                    return;
                }
            }
            JOptionPane.showMessageDialog(this, "Total da compra: R$ " + String.format("%.2f", total));
        }
    }

    public static void main(String[] args) {
        new DocesGridLayout();
    }
}
