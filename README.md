package com.example.tarunkumar.myapplication;

import android.content.Context;
import android.content.Intent;
import android.net.Uri;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.ListView;
import android.widget.Toast;
import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    ListView listView;
    MyAdapter2 adapter2;
    Button button;
    int clickCounter=0;
    ArrayList<Uri> u = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button= (Button) findViewById(R.id.button);
        listView = (ListView) findViewById(R.id.list);

        adapter2 = new MyAdapter2(this,u);
        listView.setAdapter(adapter2);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_PICK);
                intent.setType("image/*");
                startActivityForResult(intent, 1);
            }
        });
    }

    @Override
    protected void onActivityResult(int reqCode, int resultCode, Intent data) {
        super.onActivityResult(reqCode, resultCode, data);

        if (resultCode == RESULT_OK) {
            Uri imageUri = data.getData();
            u.add(imageUri);
            adapter2.notifyDataSetChanged();
            clickCounter++;
        }
        else {
            Toast.makeText(this, "You haven't picked Image",Toast.LENGTH_SHORT).show();
        }
    }

    class MyAdapter2 extends ArrayAdapter<String> {
        Context context;
        ArrayList<Uri> uriArrayList;

        MyAdapter2(Context c, ArrayList<Uri> uris) {
            super(c, R.layout.singlerow);
            this.context = c;
            this.uriArrayList=uris;
        }

        @NonNull
        @Override
        public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {

            LayoutInflater inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            View row = inflater.inflate(R.layout.singlerow, parent, false);

            ImageView myImage = (ImageView) row.findViewById(R.id.imageView);

            myImage.setImageURI(uriArrayList.get(clickCounter));

            return row;
        }
    }
}
