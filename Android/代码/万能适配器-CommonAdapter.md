# 万能适配器-CommonAdapter

## ViewHolder

```java
import android.content.Context;
import android.util.SparseArray;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by Kevin on 2016/9/25.
 * Description：
 */
public class ViewHolder {

    private SparseArray<View> views;
    private int position;
    private View convertView;

    public ViewHolder(Context context, ViewGroup parent, int layoutId, int position) {
        this.position = position;
        this.views = new SparseArray<View>();
        convertView = LayoutInflater.from(context).inflate(layoutId, null);
        convertView.setTag(this);
    }

    public static ViewHolder get(Context context, View convertView, ViewGroup parent, int layoutId, int position) {
        if (convertView == null) {
            return new ViewHolder(context, parent, layoutId, position);
        } else {
            ViewHolder holder = (ViewHolder) convertView.getTag();
            holder.position = position;
            return holder;
        }
    }

    public <T extends View> T getView(int viewId, Class<T> c) {
        View view = views.get(viewId);

        if (view == null) {
            view = convertView.findViewById(viewId);
            views.put(viewId, view);
        }

        return (T) view;
    }

    public ViewHolder setText(int viewId, String text) {
        TextView textView = getView(viewId, TextView.class);
        textView.setText(text);
        return this;
    }

    public View getConvertView() {
        return convertView;
    }
}
```

## CommonAdapter

```java
import android.content.Context;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;

import com.oaec.adapterdemo.R;

import java.util.List;

/**
 * Created by Kevin on 2016/9/25.
 * Description：
 */
public abstract class CommonAdapter<T> extends BaseAdapter {

    protected Context context;
    protected List<T> list;
  	protected int layoutId;

    public CommonAdapter(Context context, List<T> list) {
        this.context = context;
        this.list = list;
    }

    @Override
    public int getCount() {
        return list.size();
    }

    @Override
    public T getItem(int position) {
        return list.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder = ViewHolder.get(context, convertView, parent, layoutId, position);
        convert(holder, list.get(position));
        return holder.getConvertView();
    }

    public abstract void convert(ViewHolder holder, T t);
}
```

## 使用

```java
import android.content.Context;

import com.oaec.adapterdemo.utils.CommonAdapter;
import com.oaec.adapterdemo.utils.ViewHolder;

import java.util.List;

/**
 * Created by Kevin on 2016/9/25.
 * Description：
 */
public class MyAdapter extends CommonAdapter<Bean> {


    public MyAdapter(Context context, List<Bean> list, int layoutId) {
        super(context, list, layoutId);
    }

    @Override
    public void convert(ViewHolder holder, Bean bean) {
        holder.setText(R.id.tv_title,bean.getTitle())
                .setText(R.id.tv_desc,bean.getDesc());
    }

}
```

